---
layout: entry
title:  "Toggle switch"
description: "How to code and test an accessible toggle switch for Web"
categories: form

keyboard:
  tab: |
    Focus visibly moves to the switch
  spacebar: |
    Toggles the switch between states

mobile:
  swipe: |
    Focus moves to the element, expresses its state
  doubletap: |
    Element toggles between states.

screenreader:
  name:  |
    Its label and purpose is clear
  role:  |
    It identifies its role of switch, toggle button or checkbox
  group: |
    Hints or errors are read after the label and related inputs include a group name (ex: Account settings)
  state: |
    It expresses its state (on/off, checked/unchecked, disabled/dimmed)

gherkin-keyboard: 
  - when:  |
      the tab key to move focus to a switch
    result: |
      focus is strongly visually indicated
  - then:  |
      the spacebar to activate the switch
    result: |
      the state is changed

gherkin-mobile:
  - when:  |
      swipe to focus on a switch input
  - then:  |
      doubletap with the switch in focus
    result: |
      the state is changed

wcag:
  - name: Perceivable
    list:
      - criteria: Is easy to identify as a toggle
      - criteria: Color is not used as the only means of conveying state (on/off/checked/unchecked)
  - name: Operable
    list:
      - criteria: Is keyboard operable
      - criteria: The click/tap target area includes the label and is no smaller than 44x44px
      - criteria: The checked, disabled and focus states have a 3:1 minimum contrast ratio against default
      - criteria: The focus indication has a 3:1 minimum contrast ratio against adjacent elements
      - criteria: The focus indication has a minimum area equal to the width of the element and 2px in height
  - name: Understandable
    list:
      - criteria: Its purpose is clear in the context of the whole page
  - name: Robust
    list:
      - criteria: Conveys the correct semantic role
      - criteria: Expresses its state (and group name if applicable)
      - criteria: Meets criteria across platforms, devices and viewports
---

## Code examples

### Use as much semantic HTML as possible

- This semantic HTML contains all accessibility features by default, and only requires the addition of `role="switch"`. 
- It uses [CSS pseudo attributes](https://github.com/tmobile/magentaA11y/blob/main/_sass/modules/_input-switch.scss) to create the toggle switch indicator, no Javascript

{% highlight html %}
{% include /examples/input-switch.html %}
{% endhighlight %}

{::nomarkdown}
<example>
{% include /examples/input-switch.html %}
</example>
{:/}

### Disabled and focusable

If it's helpful for screenreaders to perceive a disabled toggle, use `aria-disabled="true"` and prevent click events with scripting.

{% highlight html %}
{% include /examples/input-switch-disabled-focusable.html %}
{% endhighlight %}

{::nomarkdown}
<example>
{% include /examples/input-switch-disabled-focusable.html %}
</example>
{:/}

### Disabled and not focusable

Using the `disabled` attribute will prevent the input from being clickable, but will also prevent it from being focusable, making it more difficult to discover for screenreaders.

{% highlight html %}
<input type="checkbox"
        role="switch"
        id="deltaSwitch"
        disabled
        checked>
<label for="deltaSwitch">Delta</label>
{% endhighlight %}

### Using a `<button>` as a switch

This `<button>` toggle has focus and keyboard criteria built in. It requires the addition of `role="switch"` and scripting to toggle `aria-checked="true/false"`.

{% highlight html %}
<button role="switch" aria-checked="true">
  Alpha
</button>
{% endhighlight %}

### When you can't use semantic HTML

This custom switch requires extra attributes and keyboard event listeners.

{% highlight html %}
<div role="switch" tabindex="0" aria-checked="true">
  Alpha
</div>
{% endhighlight %}

## Developer notes

### Name

- `label` text should describe the input purpose
- Use `aria-describedby="hint-id"` for hints or additional descriptions
- `aria-label="Switch purpose"` can also be used (as a last resort)

### Role

- Use `role="switch"`

### Group

- Semantic HTML
    - `<fieldset>` should wrap a switch group
    - `<legend>` should describe the group's purpose
    - Each `<label>` must include `for="input-id"` to be associated with its input
- Custom elements
    - Use `role="group"` in the place of fieldset
    - Use `aria-labelledby="label-id"` to associate an element as a label
    - `aria-label="Group purpose"` can also be used if there's no label with an ID

### State

- Semantic HTML
    - Use `checked` for native HTML
    - Use the `disabled` state for inactive switches
- Custom element
    - Use `aria-checked="true/false"` to express state
    - Use `aria-disabled="true"` to declare inactive elements
    - Use `aria-readonly="true"` to declare a switch can't be edited

### Focus

- Focus must be visible
