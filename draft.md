### Path Params
Sometimes there are variables within a pathname that we want to capture. For example, with our player profile route, we want to capture the player’s number. We can do this by adding path params to our route’s path string.



The :number part of the path /roster/:number means that the part of the pathname that comes after /roster/ will be captured and stored as match.params.number. For example, the pathname /roster/6 will generate a params object :

{ number: '6' } // note that the captured value is a string
The `<Player>` component can use the props.match.params object to determine which player’s data should be rendered.



```jsx
// an API that returns a player object
import PlayerAPI from './PlayerAPI'
const Player = (props) => {
  const player = PlayerAPI.get(
    parseInt(props.match.params.number, 10)
  )
  if (!player) {
    return <div>Sorry, but the player was not found</div>
  }
  return (
    <div>
      <h1>{player.name} (#{player.number})</h1>
      <h2>{player.position}</h2>
    </div>
)

```

You can learn more about path params in the path-to-regexp documentation.

Alongside the `<Player>` component, our website also includes` <FullRoster>`, `<Schedule>`, and `<Home>` components.


```jsx
const FullRoster = () => (
  <div>
    <ul>
      {
        PlayerAPI.all().map(p => (
          <li key={p.number}>
            <Link to={`/roster/${p.number}`}>{p.name}</Link>
          </li>
        ))
      }
    </ul>
  </div>
)
const Schedule = () => (
  <div>
    <ul>
      <li>6/5 @ Evergreens</li>
      <li>6/8 vs Kickers</li>
      <li>6/14 @ United</li>
    </ul>
  </div>
)
const Home = () => (
  <div>
    <h1>Welcome to the Tornadoes Website!</h1>
  </div>
)
```

