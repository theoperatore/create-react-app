# theoperatore-react-scripts

_Based off of react-scripts version 0.9.4 the MASTER branch_

Current version of theoperatore-react-scripts: **0.9.5**

Yet another fork of [create-react-app](https://github.com/facebookincubator/create-react-app) specifically for:

- SASS
- CSS Modules (default local)
- [enzyme-to-json](https://github.com/adriantoine/enzyme-to-json) serializer for Jest Snapshots

I basically made this so I can depend on myself to keep this fork updated with CRA's react-scripts AND keep sass support and local css module support. If you want to also use this, go nuts! If there are problems I'll try to address them as they come up.

I chose to base this fork after the `master` branch of [create-react-app](https://github.com/facebookincubator/create-react-app) because it has very nice things that I want to use now, namely webpack 2 support and a cleaner folder structure.

There is only one caveat that has presented itself so far (see the note about errors after the usage section). If you find anything else, just open an issue and I'll address it.

## Usage

enable this fork by creating a react app passing the name of this react-scripts:

```bash
$ create-react-app --scripts-version theoperatore-react-scripts my-new-react-app
```

Now you're all ready to rock!

### Quick note about an error

If you get the error while trying to `yarn start` or `npm start`:

```
Error: Cannot find module 'react-dev-utils/crashOverlay'
```

Then you need to add any version of `react-dev-utils` to your project `>0.5.2`. This project is a fork of the master branch of `create-react-app` and as such some things haven't been published yet.

This problem should resolve itself eventually with the next release of `react-dev-utils` however in the meantime, this bandage will solve it:

```bash
yarn add --dev react-dev-utils@canary
```

This will install a version of `react-dev-utils` that is compatible with this version of react-scripts.


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
