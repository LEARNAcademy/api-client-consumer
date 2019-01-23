https://player.vimeo.com/video/226597354

# Create a new React application

In this app, we're going to use the familiar ```create-react-app``` to build an app we can start working with.  We'll also add a Bootstrap package to allow us to treat Bootstrap elements as regular React components.  Its a great way to work with both technologies, and they work really well together.  Then, to give the project a bit of refinement, we'll go in search of a theme for Bootstrap.

### Steps

* ```create-react-app nasa-neo```
* ```yarn add react-bootstrap```
* Grab theme from [Bootswatch](http://www.bootswatch.com)
* Move ```bootstrap.min.css``` file from theme into ```/public``` directory
* Add ```<link rel="stylesheet" href="%PUBLIC_URL%/bootstrap.min.css">``` to ```/public/index.html```

### react-bootstrap
[website](https://react-bootstrap.github.io/getting-started.html)

Bootstrap outside of React, at its core, is a set of CSS classes applied to elements, and this makes it very easy to work with when writing HTML.  Consider the following that may be used for a typical left navigation website:

```HTML
<div class='container'>
  <div class='row'>
    <div class='col-xs-4'>
      <!-- html for left nav here -->
    </div>
    <div class='col-xs-8'>
      <!-- html for main container here -->
    </div>
  </div>
</div>
```

That works great in the world of HTML, and gives us loads of control over the look and feel of our webpage.  But, when using the React framework, we want to think about the page as a set of components.  Divs, while technically a component in React, are pretty generic ones.  Wouldn't it be nicer to write the following instead?

```HTML
<Container>
  <Row>
    <Col xs={4}>
      {/* html for left nav here */}
    </Col>
    <Col xs={8}>
      {/* html for main container here */}
    </Col>
  </Row>
</Container>
```

That's what ```react-bootstrap``` is all about.  Declarative, component syntax for Bootstrap within React.

#### Don't forget to import the components you use

A word of caution.  A very common mistake is to forget to import the Bootstrap components you use in your JSX at the top of the page.  You'll see in later videos that we add an import statement in components that use Bootstrap elements, and explicitly import each component.  Something like this:

```JS
import {
  PageHeader,
  Table
} from 'react-bootstrap'
```

All Bootstrap components are imported from 'react-bootstrap', so we can list them all out out in one import statement.

## Challenges
* Use ```create-react-app``` to build a new application about any topic you choose
* Use Yarn to add 'react-bootstrap' to your new React app
* Add a Theme to your React app.
* Import some Bootstrap components, and build a nice looking web page.
