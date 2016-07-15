---
layout: post
title:  "C 语言编译器优化导致程序行为完全不同的一个简单例子"
date:   2016-07-12 Tue 02:41:05
categories: programming
---

看这段简单但不完全平凡的代码：

{% highlight c linenos=table %}
// Filename: 0.c
void f(int n) {
    f(++n);
}

int main() {
    int i = 0;
    for(;;++i) {
        f(i*i);
    }
}
{% endhighlight %}

如果用 `clang 0.c` 编译，程序会很快因为 `f` 这个函数无限递归导致栈溢出而终止。

如果用 `clang -O3 0.c` 编译，则程序会一直停留在死循环里，不会终止。

未经优化得到的汇编代码是

{% highlight asm linenos=table %}
	.section	__TEXT,__text,regular,pure_instructions
	.macosx_version_min 10, 11
	.globl	_f
	.align	4, 0x90
_f:                                     ## @f
	.cfi_startproc
## BB#0:
	pushq	%rbp
Ltmp0:
	.cfi_def_cfa_offset 16
Ltmp1:
	.cfi_offset %rbp, -16
	movq	%rsp, %rbp
Ltmp2:
	.cfi_def_cfa_register %rbp
	subq	$16, %rsp
	movl	%edi, -4(%rbp)
	movl	-4(%rbp), %edi
	addl	$1, %edi
	movl	%edi, -4(%rbp)
	callq	_f
	addq	$16, %rsp
	popq	%rbp
	retq
	.cfi_endproc

	.globl	_main
	.align	4, 0x90
_main:                                  ## @main
	.cfi_startproc
## BB#0:
	pushq	%rbp
Ltmp3:
	.cfi_def_cfa_offset 16
Ltmp4:
	.cfi_offset %rbp, -16
	movq	%rsp, %rbp
Ltmp5:
	.cfi_def_cfa_register %rbp
	subq	$16, %rsp
	movl	$0, -4(%rbp)
	movl	$0, -8(%rbp)
LBB1_1:                                 ## =>This Inner Loop Header: Depth=1
	movl	-8(%rbp), %eax
	imull	-8(%rbp), %eax
	movl	%eax, %edi
	callq	_f
## BB#2:                                ##   in Loop: Header=BB1_1 Depth=1
	movl	-8(%rbp), %eax
	addl	$1, %eax
	movl	%eax, -8(%rbp)
	jmp	LBB1_1
	.cfi_endproc


.subsections_via_symbols
{% endhighlight %}

`O3` 优化后对应的汇编代码是

{% highlight asm linenos=table %}
	.section	__TEXT,__text,regular,pure_instructions
	.macosx_version_min 10, 11
	.globl	_f
	.align	4, 0x90
_f:                                     ## @f
	.cfi_startproc
## BB#0:
	pushq	%rbp
Ltmp0:
	.cfi_def_cfa_offset 16
Ltmp1:
	.cfi_offset %rbp, -16
	movq	%rsp, %rbp
Ltmp2:
	.cfi_def_cfa_register %rbp
	popq	%rbp
	retq
	.cfi_endproc

	.globl	_main
	.align	4, 0x90
_main:                                  ## @main
	.cfi_startproc
## BB#0:
	pushq	%rbp
Ltmp3:
	.cfi_def_cfa_offset 16
Ltmp4:
	.cfi_offset %rbp, -16
	movq	%rsp, %rbp
Ltmp5:
	.cfi_def_cfa_register %rbp
	.align	4, 0x90
LBB1_1:                                 ## =>This Inner Loop Header: Depth=1
	jmp	LBB1_1
	.cfi_endproc


.subsections_via_symbols
{% endhighlight %}

可见优化的版本里最后执行的是一个简单的死循环 (34-35 行)，对函数 `f` 的调用被完全移除了，尽管还是“假模假式”地定义了 `f` (什么功能都没有，也没有递归调用的部分)。

至于 `gcc (clang)` 是怎么推断出 `f` 这个函数虽然在做事，但做的事是“封闭的”、对外界没有“逻辑上的”副作用 (尽管有事实上的副作用，那就是栈溢出)，可以在[这里](http://llvm.org/docs/Passes.html)看到一些端倪。想了想，也许是通过把递归转换为循环可以看出 `f` 这个函数没有副作用，所以就移除了。

那个页面提到 `LLVM` 会为了方便后续分析而把
{% highlight c linenos=table %}
for (i = 7; i*i < 1000; ++i)
{% endhighlight %}
变成
{% highlight c linenos=table %}
for (i = 0; i != 25; ++i)
{% endhighlight %}
真没想到它会做这样的变换。
