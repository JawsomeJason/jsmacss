# Patterns

## What Goes in Here?

This folder holds reusable patterns.  There are two kinds:

## Placeholders (%)

See http://sass-lang.com/docs/yardoc/file.SASS_REFERENCE.html#placeholder_selectors_ for more documentation

Placeholders allow you to specify a certain non-semantic, reusable style /without/ it actually being exposed to the CSS unless it is @extend'd upon.

### Example:

    %half-width-and-centered {
        width: 50%;
        margin: 0 auto;
    }



    .cart {
        // look, we're being object-oriented and semantic
        @extend %half-width-and-centered;
    }

    .button {
        @extend %half-width-and-centered;
        background-color: orange;
    }

### Output

    .cart,
    .button {
        width: 50%;
    }

    .button {
        background-color: orange;
    }

As you can see, the selectors get combined with the common placeholder, and the button is still given a different background color.

Placeholders should actually ALWAYS be accompanied by a Mixin, which we'll show shortly.



## Mixins

See http://sass-lang.com/docs/yardoc/file.SASS_REFERENCE.html#mixins for documentation

Mixins allow us to write a function that takes in arguments and outputs custom CSS for that call.

### Example
    @mixin half-width-and-centered( $background-color: false ) {
        width: 50%;
        margin: 0 auto;
        @if $background-color != 'false' {
            background-color: $background-color;
        }
    }

    // same selectors as before, but with mixins that allow us to send in arguments
    .cart {
        @include half-width-and-centered;
    }

    .button {
        @include half-width-and-centered( orange );
    }

### Output

    .cart {
        width: 50%;
        margin: 0 auto;
    }

    .button {
        width: 50%;
        margin: 0 auto;
        background-color: orange;
    }
## OH NO!  Best of Both Worlds?
Looks like mixins can kind-of suck, because they duplicate code.  But the parameter part is so cool!  Let's combine the patterns.

### Example

%half-width-and-centered {
    width: 50%;
    margin: 0 auto;
}

@mixin half-width-and-centered( $background-color: false ) {
    // static, reusable part
    @extend %half-width-and-centered;

    // custom logic
    @if $background-color != 'false' {
        background-color: $background-color;
    }
}

    .cart {
        @include half-width-and-centered;
    }

    .button {
        @include half-width-and-centered( orange );
    }

### Output
    .cart,
    .button {
        width: 50%;
    }

    .button {
        background-color: orange;
    }

Wow, looks like we have the conciseness of Placeholders with the extensibility of Mixins.  Win/Win!

Good rule of thumb: *Always throw

## @import-once

Be sure to check out ../functions/_import-once.scss.  Anytime you use Placeholders, also add an identical Mixin that extends the Placeholder.  In usage, NEVER extend the placeholder elsewhere in your code.  Instead, include the Mixin.  That way, you can keep adding logic as you need for one include without having to refactor others.  Plus, you get the combining of the Placeholder!