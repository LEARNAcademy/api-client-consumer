https://player.vimeo.com/video/226597010

# Table Data

Now that we have an array of Asteroids in the component's state, we're ready to display the data in our table.

## Iterating over an array in JSX

JSX is a powerful tool to build complex HTML structures.  JSX is actually just Javascript that allows HTML to be mixed in, so we can loop, add conditionals, and do many other logical things to our markup.  In this case, we're going to be looping over an array:

```Javascript
{this.state.asteroids.map((asteroid)=>{
  return(
    .. any HTML we want
  )
}
```
This structure renders the array of HTML markup objects returned from the map operation on our array.

Here is the complete table:
```Javascript
<Table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Estimated Diameter (feet)</th>
      <th>Date of Closest Approach</th>
      <th>Distance (miles)</th>
      <th>Velocity (miles/hour)</th>
    </tr>
  </thead>
  <tbody>
    {this.state.asteroids.map((asteroid)=>{
      return(
        <tr key={asteroid.id}>
          <td>{asteroid.name}</td>
          <td>{asteroid.diameterMin} - {asteroid.diameterMax}</td>
          <td>{asteroid.date}</td>
          <td>{asteroid.distance}</td>
          <td>{asteroid.velocity}</td>
        </tr>
      )
    })}
  </tbody>
</Table>
```

## Challenges
* Create a new React app (or reuse one from previous challenges) and load sample data from the NEO api
* Iterate over the mapped data and represent it as a table on your web page
