# Component development Guildline

## Table of Content

## Overview
In this guideline we'll go through a process step by step to implement a component from scratch, from where we pick a task to finish it.

For this guideline we assume that we want to implement a Button component for our library.

Let's dive into it.

## Pick the task
To start implementation of a component we should go to projects board in the github organization and in the related board you can pick any unassigned task in `ready` column.

> You can check react project board in this [link](https://github.com/orgs/UI-Library-Lab/projects/1/views/1).

On clicking each task you can check all descriptions and acceptance criterias of task which is the all requirements for admission of this task.

After choosing one task you can assign it to yourself and move it to `In Progress` column.

## Making branch
In this step picking our task we need to make a branch based on `develop` branch.

You can go to `develop` branch and pull last changes of develop from origin.

Now we should make a new branch based on contribution rules.

> To find how to name a branch for contribution, please check our [CONTRIBUTING.md](./../../CONTRIBUTING.md) .

In this example as we want to implement a button and it's a feature we name our branch as `feature/button`.

## Check The design and requirements
In the task's descriptions you can find a link for related design for this component. on click of it, it leads you to the corresponding design.

Now we want to know how to check component properties to know about all requirements for our component.


## Implement the component
Finally we reached to the step, where we want to implement our component.


### Folder selection
First of we should find a proper place to start our implementation. Based on [full-documentation](./../full-documentation/full-documentation.md) file all of our components categorized based on their functionality which you can see in that documentation file.

This type of component has written in task description in the board. So we can go to `components` folder, then based on type of component in the task description we can choose its folder and if it not exists we can create a folder for that category and inside of it we can make our folder for our component. 

So based on this details our Button component location will be:

`src/components/inputs/button`

> Note: don't forget to follow the file and folder naming convention which is kebab-case.

### Required files
Now we're in component folder and we wanna make required files to implement our component.

Each component we can have one folder and four files:

1. story folder: in this folder we put our story template file
2. button.tsx: this is the file where our component implementation located.
3. style.ts: as its name says, it contains all styles related to this component.
4. props.ts: which is our interface for specifing the properties for our component.
5. use-logics.ts: which is a custom hook to handle ui logics that its optional and add it just when you need some ui logics in this component.

### Props interface
The first step is to specify interface for our component to know which properties will come to our component. to know about these properties we need to look at the task descriptions and in the design properties you can see list of properties which this component needs.

So based on that you can make interface and needed enums for your component.

```ts
// ~/components/inputs/button/props.ts
import ICommonProps from '~/components/common/common-props';
import { Sizes } from '~/components/common/common-enums';
import { ReactNode } from 'react';

/* ---------------------------------- Enums --------------------------------- */
export enum ButtonVariants {
  PRIMARY = 'Primary',
  SECONDARY = 'Secondary',
  TERTIARY = 'Tertiary',
  TERTIARY_GRAY = 'Tertiary Gray',
  GHOST = 'Ghost',
  GHOST_GRAY = 'Ghost Gray',
  LINK = 'Link',
  LINK_GRAY = 'Link Gray',
}

/* -------------------------------- Interface ------------------------------- */
export interface ButtonProps extends CommonProps<HTMLButtonElement> {
  /**
   * Type of button to show
   *
   * Default: Primary
   */
  variants?: `${ButtonVariants}`;
  /**
   * How large should the button be?
   *
   * Default: MEDIUM
   */
  size: `${Sizes}`;
  /**
   * Button contents
   */
  label: string;
  /**
   * Icon to show before button
   */
  Leading?: ReactNode;
  /**
   * Icon to show after button
   */
  Trailing?: ReactNode;
  /**
   * Is error shape
   */
  destructive?: boolean;
  /**
   * Will take all width spaces
   */
  expanded?: boolean;
  /**
   * Is button disabled?
   */
  disabled?: boolean;
}
```

> Note: For using same comment divider like this one for separating enums from interfaces and each parts of a file from each other you can use [comment-divider](https://marketplace.visualstudio.com/items?itemName=stackbreak.comment-divider) plugin of vscode.

As you can see for our button component we made a `ButtonProps` interface which has all props that is needed for our component with `JSDOC` for each one of them to describe what is it and what it does.

This props will be used directly by the users which will use this component.

Also we Extended from `CommonProps` interface like other components of our library as all of them should extend from it and we passed our component html type as a generic type to this `CommonProps` which here it is `HTMLButtonElement`.

> Note: Some enums can be used in any components and they are general. For such enums we put them in `~/components/common/common-props`. You can see we put our size enum to this file and we imported here as `Size` enum can be same for many components and it'll be better to implemented once for all of them as a single source of truth. 

If you had an enum which can be used for many components, put it in that directory with a `JSDOC` to describe what is it. like this one.

```ts
// ~/components/common/common-enums.ts
/**
 * You can use this enum as Size enum for you component props.
 */
export enum Sizes {
  SMALL = 'Small',
  MEDIUM = 'Medium',
  LARGE = 'Large',
  XLARGE = 'XLarge',
  XXLARGE = 'XXLarge',
}
```

### Component file
After specifying interface for props. we can go for implementing our main component. for choosing file name for our component we're using `kebab-case` like other files, with the name of component itself. 

So in our example it'll be `button.tsx`.

Then inside of this file we'll implement our functional react component for our project.

```ts
import { type ForwardedRef } from 'react';
import withThemeWrapper from '~/theme/with-theme-wrapper';
import { type IButtonProps } from './i-button-props';
import { StyledButton } from './style';
/* -------------------------------------------------------------------------- */
/**
 * Primary UI component for user interaction
 */
const Button = withThemeWrapper<ButtonProps>(
  (props: ButtonProps, ref: ForwardedRef<HTMLButtonElement>) => {
    const { label, Leading, Trailing } = props;
    return (
      <StyledButton {...props} ref={ref}>
        {Leading}
        {label}
        {Trailing}
      </StyledButton>
    );
  },
);

export default Button;

```

As you see we should wrap all of our components with `withThemeWrapper` which handles passing theme tokens to our components and also forwarding the ref as well.

Also we use `JSDOC` for each component to describe what is this component will be used for.

Here we can implement our component based on the design and passing props.

Also we're using `styled-components` for styling components and we're passing props to it to styling based on passing props, which will be located in `style.ts` file and we'll describe it in the next step.

### Styling
As we said in `style.ts` file we'll do our styling for our component based on passing props.

In each styled component, we can access to two props:
1. theme: which is our design tokens
2. all props: which is our component properties that we passed to this component by ourself.

For our example for this `StyledButton` you can access `theme` and `props` like this:

```ts
export const StyledButton = styled.button<IStyledButton>`
  border-radius: ${({ theme }) => theme.spacing[1]};
  cursor: ${({ disabled }) => (disabled ? 'auto' : 'pointer')};
`;
```

As you see `disabled` was one of our button props and `theme` is our global theme token which we should use it for styling based on passing props.

> Note: Avoid using static data for styling and we should use `theme` properties for styling. You can check it's type by hovering on it.

> Note: All styled components name will be start with `Styled`.

Our final shape of styled component for our `StyledButton` will be like this:

```ts
// ~/components/inputs/button/style.ts
import styled, {
  DefaultTheme,
  FlattenInterpolation,
  ThemedStyledProps,
  css,
} from 'styled-components';
import { Sizes } from '~/components/common/common-enums';
import {
  commonTransition,
  commonTypography,
} from '~/components/common/common-styles';
import {
  ButtonVariants,
  IButtonProps,
} from '~/components/inputs/button/i-button-props';
import {
  type NTypographyConfigs,
  type SpacingConfigs,
} from '@ui-library-lab/core-js';

/* ---------------------------- Styled Components --------------------------- */
export const StyledButton = styled.button<IStyledButton>`
  ${commonTypography}
  ${commonTransition}
  // Sizes css
  ${buttonSizesStyles}
  // types css
  ${({ variants, destructive }) =>
    variants && buttonTypeStyle(destructive || false)[variants]}
  border-radius: ${({ theme }) => theme.spacing[1]};
  cursor: ${({ disabled }) => (disabled ? 'auto' : 'pointer')};
  display: flex;
  justify-content: center;
  align-items: center;
  gap: ${({ theme }) => theme.spacing[3]};
`;
```

As you see we didn't specify all of our logics in one styled component and we separate it to `commonTypography`, `commonTransition`, `buttonSizesStyles` and `buttonTypeStyle` because they needed some logics that based on props we should specify our css details and we don't want to make this `StyledComponent` messy by handling all props logics in one place so we separated `css`s in a separate `css` value. 

> Note: `commonTypography` and `commonTransition` are some common styles that can be used in many components so we put them in `~/components/common/common-styles.ts`, If you had such styles you can put them there as well. which will be like this:
```ts
// ~/components/common/common-styles.ts
import { css } from 'styled-components';

/**
 * You can use it for common font related styled in your components
 */
export const commonTypography = css`
  font-family: ${({ theme }) => theme.typography.fontFamily};
  font-size: ${({ theme }) => theme.typography.fontSize.text.md.size};
  font-weight: ${({ theme }) => theme.typography.fontWight.medium};
  line-height: ${({ theme }) => theme.typography.fontSize.text.md.lineHeight};
`;

/**
 * Shared transition css rule for all component shoud use this one
 */
export const commonTransition = css`
  transition: all 0.2s;
`;

```

For `buttonSizesStyles` it'll be in the same `style.ts` file like this:
```ts
const buttonWidthSizesStyle = (sizes: SpacingConfigs) =>
  ({
    [Sizes.SMALL]: sizes[32],
    [Sizes.MEDIUM]: sizes[34],
    [Sizes.LARGE]: sizes[36],
    [Sizes.XLARGE]: sizes[38],
    [Sizes.XXLARGE]: sizes[48],
  } as Record<`${Sizes}`, string>);

const buttonHeightSizesStyle = (sizes: SpacingConfigs) =>
  ({
    [Sizes.SMALL]: sizes[8],
    [Sizes.MEDIUM]: sizes[10],
    [Sizes.LARGE]: sizes[11],
    [Sizes.XLARGE]: sizes[12],
    [Sizes.XXLARGE]: sizes[14],
  } as Record<`${Sizes}`, string>);

const buttonFontSizes = (font: NTypographyConfigs.Typography) =>
  ({
    [Sizes.SMALL]: font.fontSize.text.sm.size,
    [Sizes.MEDIUM]: font.fontSize.text.sm.size,
    [Sizes.LARGE]: font.fontSize.text.md.size,
    [Sizes.XLARGE]: font.fontSize.text.md.size,
    [Sizes.XXLARGE]: font.fontSize.text.lg.size,
  } as Record<`${Sizes}`, string>);


const buttonSizesStyles = css<IStyledButton>`
  min-width: ${({ size, theme, expanded }) =>
    expanded ? '100%' : buttonWidthSizesStyle(theme.spacing)[size]};
  min-height: ${({ size, theme }) =>
    buttonHeightSizesStyle(theme.spacing)[size]};
  font-size: ${({ size, theme }) => buttonFontSizes(theme.typography)[size]};
  line-height: ${({ size, theme }) => buttonLineHeight(theme.typography)[size]};
  padding: ${({ theme, size }) => buttonPadding(theme.spacing)[size]};
`;
```

As you see we separated `css` styling for `Size` property and in this css styling we handled all size related styling rules and even for each property we made a separate object to choose correct `theme value` based on passing `size` property.

And we did the same thing for `variant` property as well. because it needed some logics to decide styling for our component based on passing `variant` properties.

So you saw for each properties we made a separate `css` and connect them and just calling in `StyledButton` as a last part which will be called in our button.

> Note: For separating styles there is no strict rules but try to separate them based on each `prop` and some `common` styles. feel free for separation based on your component situation and requirements, the main concept is to separate concepts and avoid our styles to get messy and make them reusable as much as possible.

### Storybook
As we're developing our button we need to test and check our button UI based on passing props. in story folder of your component you can make story file to add templates for your component. name of our component will be `button.stories.tsx`.

```ts
// ~/components/inputs/button/story/button.stories.tsx
import { Meta, StoryObj } from '@storybook/react';
import {
  ButtonVariants,
  IButtonProps,
} from '~/components/inputs/button/i-button-props';
import { Sizes } from '~/components/common/common-enums';
import { Circle } from '~/components/icons';
import { Button } from '~/index';

export default {
  title: 'Input/Button',
  parameters: {
    design: {
      type: 'figma',
      url: 'https://www.figma.com/file/DD3GQCNDXjQaaXApTxmfES/Jinex-Design-System-(Code)?type=design&node-id=2-18000&mode=design&t=dhI8p1bAV000t9J2-4',
    },
  },
  args: {
    label: 'Default Label',
    destructive: false,
    variants: ButtonVariants.PRIMARY,
    size: Sizes.MEDIUM,
    disabled: false,
    expanded: false,
  },
  argTypes: {
    disabled: {
      control: 'boolean',
    },
    variants: {
      control: 'radio',
      options: Object.values(ButtonVariants),
    },
    size: {
      control: 'radio',
      options: Object.values(Sizes),
    },
    label: {
      type: 'string',
    },
    destructive: {
      defaultValue: false,
      description: 'Danger state or not',
      type: 'boolean',
    },
    expanded: {
      defaultValue: false,
      description: 'take all width places of parent',
      type: 'boolean',
    },
  },
} as Meta<typeof Button>;

export const Primary: StoryObj<Meta> = {
  render: (props) => {
    return (
      <Button
        {...(props as IButtonProps)}
        Leading={<Circle />}
        Trailing={<Circle />}
      />
    );
  },
};

```

As you see we in parameters and design we put related design link for this component in our figma.

Also in argTypes we specified property types and description of props that we specified in prop interface to check them in storybook. in this step we should specify args for all properties which we need to be documented completely and make sure they're completely working in the way that we want.

## Pull request 
After implementing all of properties and style logics which mentioned in the task description and the design, we can make pull request from our branch which is `feature/button` in this example to `develop` branch and we should wait for pipline to check if everything is ok with our branch and move our task in the board to `In review` column and wait for reviewing. If we got any request changes from other reviewers we should fix the problem and again ask for rereview again and repeat this process till our pull request will be addmited and merge with develop branch.

And that's it. Congratulations you've already contributed to our community and added your first component to our component library.

Thanks for contributing. We really appreciate it.