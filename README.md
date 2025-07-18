# Inkling Script Language

Inkling is a scripting language inspired by [Ink](https://www.inklestudios.com/ink/) by Inkle Studios. While it has similar syntax, the features it provides are much more limited to those of Ink as Inkling was originally designed as a way to write interactive text message conversations that could be pasted into HTML editors on fan fiction sites.

[See it in action here](https://codepen.io/gingerchew/pen/myJNaZM/91484ea1b7bfed643b4683753950177e)

## Writing your script

> For the purposes of demonstrating everything Inkling does, we will assume you are writing an interactive fic that uses text messages as the plot delivery device.

Every Inkling script starts the same way:

```
= start
```

> The space is optional, but preferred

Next, have someone say something:

```
= start
Hey there!
```

Every line that doesn't have a symbol in front of it will be treated as a secondary unnamed party talking to the owner of the phone. You can be more explicit about this by starting the line with a `>` character.

We should respond:

```
= start
Hey there!

- What's up?
- Hey! I was just thinking about you!
```

By starting a line with a `-` this tells Inkling you are creating an option. If you want to have the owner write a message without an option, start a line of text with a `<` character.

```
= start
Hey there!
< Hey!

- What's up?
- I was just thinking about you!
```
Options are nothing without a destination though. Those can be chosen by adding `-> {section_id}` after the option text.

```
= start
Hey there!

- What's up? -> text_1
- I was just thinking about you! -> textTwo
- Oh great, you again -> text-3
```

Section IDs only have two requirements, first they must not contain spaces, second they must be unique.
Notice how the section IDs accept pascal, camel, and kebab case? You can name your sections anything you want, but they cannot include spaces. You can name your sections something simple, like `A` or `next`, but it would benefit you to be a bit more verbose depending on the length of your script. Now that we have our options and our destinations, let's add to the conversation.

```
= start
Hey there!

- What's up? -> text_1
- I was just thinking about you! -> textTwo
- Oh great, you again -> text-3

= text_1
Not much!

= textTwo
Really? I'm blushing!

= text-3
Well, forget you then!
```

Inkling doesn't worry whether it has been sent to the end of a script or not. If there are no options, then the script is over.

You might want to leave yourself some comments though, just so you don't lose your place!

```
= start
Hey there!

- What's up? -> text_1
- I was just thinking about you! -> textTwo
- Oh great, you again -> text-3

# What a milque toast response
= text_1
Not much!

# This is the good out come
= textTwo
Really? I'm blushing!

# They blew it
= text-3
Well, forget you then!
```

Start a line with a `#` and that line will be ignored by the compiler. Currently inline comments or comments at the end of lines are not supported.

But what if we want to have a group chat?

```
= start
>:Maria: Hey y'all!
>:Jane: Sup
>:Elizabeth: How are you today?

- Fine, chemistry is kicking my butt though -> A
- Ugh, regretting not studying -> B

= A
>:Elizabeth: Agreed, what is a mole anyways?
>:Jane: It's mol you clod!
< Don't call her a clod

= B
>:Maria: We should have a study slumber party!
>:Jane: I'd rather sleep in my own bed, thank you
< Agreed
```

We've added to the initial symbols that can be used. You have already seen `>` and `<` but we've now introduced labeling. Labeling allows for assigning names to a given sender. Simply mark it as a received text `>` Then add the name wrapped in `:` colons.

```
>:Jane: Like this
>:Elizabeth's Whole Name Is Longer: This technically works, but is a hassle.
>:Maria This is incorrect since it will think this whole string is the name
```

There might be information you use through out your work that you want to keep on hand. This can be anything you want it to be, but ultimately it will be coerced into a string.

```
~ mc_age = 23
~ mc_name = Jeremiah Berbiglia Bartholomew Longname
> Wait {mc_name}, how old are you again?
< I'm {mc_age}
```

Variables act as constants that can be interpolated into your work. There is no way to modify them over the course of a story. It is also not possible to derive or calculate the value of a variable during assignment.

```
~ mc_age = 21 + 5 # invalid
```

## Differences From Ink

Ink is a fully featured language that allows for things like variables, cycling through options, including multiple files to create one cohesive compiled final product, or returning to an option set multiple time. You can use knots and stitches to organize sections and subsections of text. You can use `-> section_id` to send the reader to a specific knot/stitch.

Inkling is not designed for multi-file project, does not keep track of variables, does not allow for returning to previously selected options, cannot cycle through text randomly. While it has the concept of sectioning text, it does not support the `knot.stitch` dot notation or have a `=== knot name ===` equivalent. There are only stitches. `-> section_id` is only valid inside of an option.

This is why we say that Inkling is inspired by Ink and has a lot of syntactical similarities.

## Symbol Table

| Symbol | Purpose                                                                                               | Example                                                |
|--------|-------------------------------------------------------------------------------------------------------|--------------------------------------------------------|
| =      | Marks the start of a section. Must be followed by a unique string that does not contain spaces        | `= section_id`                                         |
| >      | Marks a line of text as being received                                                                | `> Hey John, it's martha`                              |
| <      | Marks a line of text as being sent                                                                    | `< Good to hear from you martha`                       |
| >:name:     | Marks a line of text as being received but is followed by a name                                      | `>:Jimothy: I don't need to tell you my name`           |
| -      | Creates an option, must have a `->` destination selector and a valid section id after the option text | `- Who added Jimothy`            |
| ->     | Marks the destination an option will lead to, must be followed by a valid section id                  | `- I forgot I added you -> removeJimothyFromGroupChat` |