# theoperatore-react-scripts

_Based off of react-scripts version 0.9.4_

Current version of theoperatore-react-scripts: **0.9.5**

Yet another fork of [create-react-app](https://github.com/facebookincubator/create-react-app) specifically for:

- SASS
- CSS Modules (default local)
- [enzyme-to-json](https://github.com/adriantoine/enzyme-to-json) serializer for Jest Snapshots

## Usage

enable this fork by creating a react app passing the name of this react-scripts:

```bash
$ create-react-app --scripts-version theoperatore-react-scripts my-new-react-app
```

Now you're all ready to rock!

## SASS Examples

Set up a sass file such as this one:

```sass
// index.scss
@import "./variables.scss";

.cool_styles {
  background-color: $primary-color;
  color: $white;
}
```

Then import it like any other module where you want to use it:

```jsx
import React from 'react';

// import your styles as javascript objects, and then reference
// them like any other object.
import styles from './index.scss';

export default function PrimaryButton({ children }) {
  return <button className={styles.cool_styles}>
    {children}
  </button>
}
```

## JEST Examples

By using this serializer, your snapshots will be smaller and to the point:

```jsx
// inside LogicalWrapper.js
import React from 'react';
import SomeOtherComponent from './SomeOtherComponent';
import Loading from './Loading';

export function LogicalWrapper(props) {
  return (props.isLoading
    ? <Loading />
    : <SomeOtherComponent />);
}
```

Now you make a snapshot test, as you would with any other jest snapshot test:

```jsx
// inside LogicalWrapper.test.js
import React from 'react';
import { shallow } from 'enzyme';
import { LogicalWrapper } from './LogicalWrapper';

test('<LogicalWrapper /> renders loading when isLoading is true', () => {
  expect(shallow(<LogicalWrapper isLoading />)).toMatchSnapshot();
});

test('<LogicalWrapper /> renders not loading when isLoading is false', () => {
  expect(shallow(<LogicalWrapper />)).toMatchSnapshot();
});
```

There is no difference in the setup, but there is a difference in the snapshot itself as the shallow renderer no longer logs the entire React object component, but rather a higher level output.

**TLDR;** It reduces the line counts of snapshots, and makes them more readable to humans. see [enzyme-to-json](https://github.com/adriantoine/enzyme-to-json) for more info.
