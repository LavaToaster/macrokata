# Exercise 7: More Repetition

This exercise is going to also cover writing repetitions, but now involving more than
one metavariable. Don't worry -- the syntax is the exact same as what you've seen before.

Before you start, let's just quickly cover the different ways you can use a metavariable
within a repetition.

## Multiple Metavariables in One Repetition

You can explicitly specify that two metavariables should be used in a single repetition.

For example, `( $($i:ident is $e:expr),+ )` would match `my_macro!(pi is 3.14, tau is 6.28)`. 
You would end up with `$i` having matched `pi` and `tau`; and `$e` having matched `3.14` and
`6.28`. 

Any repetition in the transcriber can use `$i`, or `$e`, or both within the same repetition.
So a transcriber could be `$(let $i = $e;)+`; or `let product = $($e)*+`

## One Metavariable Each, For Two Repetitions

Alternatively, you could specify two different repetitions, each containing their
own metavariable. For example, this program will construct two vecs.

``` rust
macro_rules! two_vecs {
    ($($vec1:expr),+; $($vec2:expr),+) => {
        let mut vec1 = Vec::new();
        $(vec1.push($vec1);)+
        let mut vec2 = Vec::new();
        $(vec2.push($vec2);)+
        
        (vec1, vec2)
    }
}
```

Importantly, with the above example, you have to be careful about using `$vec1` and `$vec2`
in the same repetition. It is a compiler error to use two metavariables captured 
a different number of times in the same repetition.

To quote [the reference](https://doc.rust-lang.org/reference/macros-by-example.html#transcribing):

> Each repetition in the transcriber must contain at least one metavariable to
> decide how many times to expand it. If multiple metavariables appear in the
> same repetition, they must be bound to the same number of fragments. For
> instance, `( $( $i:ident ),* ; $( $j:ident ),* ) => (( $( ($i,$j) ),* ))` must
> bind the same number of `$i` fragments as `$j` fragments. This means that invoking
> the macro with `(a, b, c; d, e, f)` is legal and expands to `((a,d), (b,e), (c,f))`,
> but `(a, b, c; d, e)` is illegal because it does not have the same
> number. 

# Starting Exercise 7

You're now ready to complete `07_more_repetition`.
Your task is to create a macro which creates a hashmap from mutiple literals and expressions.