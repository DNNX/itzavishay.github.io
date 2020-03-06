---
layout:     post
title:      Open Source Contribution
date:       2020-03-04 22:00:00
summary:    I contributed to rust-analyzer :) I'll talk about what I did and why I think you should contribute to open source software too!
categories: 
---

Hey there! Contributing to open source software is awesome. I'm going to talk about the story behind my first contribution, what I did and why I think you should contribute
to a project of your choice too, feel free to skip to your section of interest!

# My Contribution

## Why?

I have been planning on beginning a new project - a game; a clone of `Achtung, die Kurve!`. I wanted it to be written in Rust, mainly because I think the language
is really cool and the fact that I'm not skilled enough in it :) I bought a brand new laptop lately (I dislike developing on my PC), installed Ubuntu on it and set up the
developing environment.

I'm using [neovim](https://neovim.io/) as my preferable editor along with [coc](https://github.com/neoclide/coc.nvim) for advanced IDE features (such as completions, diagnostics
and more). Though, I had to pick a Rust language server[^1] to complement coc. I had two options, [RLS](https://github.com/rust-lang/rls) or [rust-analyzer](https://github.com/rust-analyzer/rust-analyzer/),
the former is the more mature and stable option but the latter is the newer, faster and more maintained option (might even call it RLS 2.0). I picked the latter.

I started the game development project (hooray!). I picked [ggez](https://github.com/ggez/ggez) as the game engine, opened the 'Hello World!' example. Of course, as I'm viciously reading and copying
the code for my enjoyment, I see the suggested completions (thank you language server!):

_![completions](/assets/demo-sad.png)_

Let's expand the conf option as it is what the tutorial uses:

_![no parameters](/assets/demo-sad2.png)_

Wait, where are the parameters? I'm pretty sure this method had parameters, I'll delete and pick it again from the completion list.. Ah! No parameters again? I probably messed up my environment,
I'll quickly google the issue and find the missing flag or misconfigured file... 

[I found a relevant issue pretty fast](https://github.com/rust-analyzer/rust-analyzer/issues/1705).
<blockquote>
  <p>
    currently, rls supports presenting completions for associated functions as snippets where you can jump through the various args.
  </p>
</blockquote>

Yay!

Wait, **RLS**?

## What?
<blockquote>
  <p>
    <b> rust-analyzer does not yet have this feature. </b> Instead it will insert a pair of parens, and place the cursor between them if the function has 1 or more args, and after if the function has no args.
  </p>
</blockquote>

That's a bummer, I don't want to switch to RLS, I set everything up already and RLS might be slower :(

Fine, I'll do it!

_![I'll bite the bullet and do it myself! (Yes, that contributor tag is a spoiler)](/assets/bite-the-bullet.png)_

## How do I do it?

Obviously, I didn't have the slightest clue about how the hell I'm going to implement this feature. The code base is huge; I never really did anything practical with Rust;
This will probably take years for me to finish..

Fortunately, [the next comment by Aleksey](https://github.com/rust-analyzer/rust-analyzer/issues/1705#issuecomment-593004414) referred me to the various relevant links that
were already posted in the issue.

Here's what I did:

 * Read through [the development documentation](https://github.com/rust-analyzer/rust-analyzer/tree/master/docs/dev) (most open source projects will have an equivalent such as `CONTRIBUTING.MD`).
 This was interesting and helpful but really overwhelming; So much information I did not know what to do with. 
 
 * Went through [the links in Jane's comment](https://github.com/rust-analyzer/rust-analyzer/issues/1705#issuecomment-522732758). But rather than glimpsing through them I tried my best
 to read and understand as much about the context presented. I found features of `rust-analyzer`, such as symbol definitions and references really useful, navigating through the code
 efficiently really improved my learning experience. After a while, I found the ['code that inserts parens'](https://github.com/rust-analyzer/rust-analyzer/blob/c7d37e424f1e04f87982233f97e1e9385191b7f1/crates/ra_ide_api/src/completion/presentation.rs#L120-L130)
 link the most relevant to the feature (note that the link is giving context for a blob in a certain commit, rather than the most updated file at the master branch).

 * Asked some questions in [the Rust zulip chat](https://rust-lang.zulipchat.com/).
 
 * Read the [LSP Specification](https://microsoft.github.io/language-server-protocol/specifications/specification-current), more specifically - [textDocument_completion](https://microsoft.github.io/language-server-protocol/specifications/specification-current/#textDocument_completion).
Thanks to that, I understood that I'll need to return a snippet in the following form:

```Rust
function_name(${1:arg_a}, ${2:arg_b})$0
```
(`arg_x` will be the name of the parameter and there will be as many as thre are in the actual function)

This will tell the editor how the snippet is built, what will be written under each snippet part and how the snippet navigation will be done. In elaboration (`|x|` notates selected text):
1. The snippet will expand to `function_name(|arg_a|, arg_b)`, writing anything will overwrite `arg_a`, if we jump to the next part of the snippet:
2. `function_name(arg_a, |arg_b|)`, and then:
3. `function_name(arg_a, arg_b)|` (`|` for the current cursor position)

Read the specification to know exactly how the format looks and what you can do with it :)

## Putting my stamp on the project

Eventually, I felt like I knew enough of what needs to be done, and the missing action was implementation. I created [my first pull request](https://github.com/rust-analyzer/rust-analyzer/pull/3432)
(I tested that it works as well, don't worry!). Aleksey reviewed and passed it. **My code is merged into master!**

# I think you should do it too!

Although scary at first, I believe every developer should try to contribute to an open source project of his choice for various reasons:

 * **It takes you out of your comfort zone and teaches you a lot.** My Rust skills were *Rusty* (heh) at best, I barely knew anything about the language server protocol, now I feel much 
 more confident in both.

 * **Many if not most of your daily software is open source and developed by contributors.** Some might say that proprietary software is better. Regardless, as a developer, I
 feel that we owe open source software (and their maintainers / contributors!) a lot, we wouldn't be where we are in terms of advancement had they worked on closed software exclusively.
 Putting my minor feature into a single open source project tenfolded my appreciation for the open source community.

 * **It is FUN!** Really, it is fun. I got to know some awesome people in the Rust zulip chat; I feel good about implementing the feature I wanted and I can't help but be happy that
 people will use and enjoy this feature (although they probably will never know who I am, and that's fine!).

 In conclusion, find a project that you frequently use that is on github, take a look in the issues tab, find something you like and ![JUST DO IT!](https://media1.giphy.com/media/b7f0X8Okk1uyk/source.gif)
 I hope you enjoyed the read, let me know about your thoughts in the comments or through reddit ([/u/itzavishay](https://www.reddit.com/user/itzavishay)), I'll be more than happy to listen or chat!

## Bonus: A bug (?) in my pull request
Right after my pull request was merged, I left home and felt that something was forgotten - what about methods?
(I tested the feature using functions that are not defined under a struct)

In Rust, methods include the `self` parameter in their declaration, 
which tells the compiler that this method is to be used in conjuction with an instance of the enclosing struct (in comparison to a static method that can be called without one).

One important detail is that using the dot notation for calling methods will exclude self from the required parameters - `instance.method(param_a, param_b)`
What did my feature do? Insert self as a suggestion, of course. I waited until I got back home, created [an issue](https://github.com/rust-analyzer/rust-analyzer/issues/3466)
and went straight into fixing the disastrous bug I introduced.

I finished the fix, added new tests, rebased over the latest master.

And... a conflict? Someone touched that code again already? Let's see..
_![did I fix it already on a different timeline?](https://user-images.githubusercontent.com/5567310/75924613-ee437300-5e6f-11ea-9436-01cc0bf66389.png)_
(My code is below, the code from master is above)

Apparently, [Aleksey fixed that already](https://github.com/rust-analyzer/rust-analyzer/pull/3442), cool guy :)

---

[^1]: Language servers are a specific-language implementation of the [language server protocol](https://langserver.org/) that allows (potentially) every editor to have advanced IDE features
