# Webpack Module Federation for React Micro-Frontends

This is a sample to elaborate on micro-frontends using a single framework for all the remote applications.

(Note: The application does not use any real data or server fetches)

### Application Structure

`container` is the host application where `dashboard` and `marketing` frontend applications are being imported and mounted.
The `mount` function is configured in remote applications to make sure the HTML are to be mounted in the right elements in the container application.

```
// marketing/src/bootstrap.js

const mount = (el) => {
  ReactDOM.render(<App />, el);
};

if (process.env.NODE_ENV === 'development') {
  const devRoot = document.querySelector('#_marketing-dev-root');

  if (devRoot) {
    mount(devRoot);
  }
}

export { mount };
```

### Why do we need `mount` function ?

One of the inflexible concept of micro-frontends is to assume that the applications are not coupled on any level with the remotes or the host. Therefore, we may not be
sure if the host contains the element with a certain "id" to which we are trying to append the remote code is present. We may need to make the mounting dynamic, so a `mount()` 
would be the standard for any micro frontend architecture.

We may need to import the `mount()` from the remote end-points in the host and make sure to convert it into a component of the framework which we are using (React.js). Here's how we convert it into a component

```
// container/src/components/MarketingApp.js

import { mount } from 'marketing/MarketingApp'
import React, {useRef, useEffect} from 'react'

export default () => {
	const ref = useRef(null);

	useEffect(() => {
		mount(ref.current);
	});

	return <div ref={ref} />
}

```

We use the [`useRef`](https://reactjs.org/docs/hooks-reference.html#useref) react hook to convert the `mount` function which we imported into `MarketingApp` component