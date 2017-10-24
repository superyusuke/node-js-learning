

## History

Each router creates a history object, which it uses to keep track of the current location[1] and re-render the website whenever that changes. The other components provided by React Router rely on having that history object available through the context, so they must be rendered inside of the router. A React Router component that does not have a router as one of its ancestors will fail to work. If you are interested in learning more about the history object (I think that this is important), you can check out my article A Little Bit of History.
Rendering a <Router>
Router components only expect to receive a single child element. To work within this limitation, it is useful to create an <App> component that renders the rest of your application (separating your application from the router is also important for server rendering because you can re-use the <App> on the server while switching to a <MemoryRouter>).
import { BrowserRouter } from 'react-router-dom'
ReactDOM.render((
  <BrowserRouter>
    <App />
  </BrowserRouter>
), document.getElementById('root'))
Now that we have chosen our router, we can start to render our actual application.