https://player.vimeo.com/video/226597455

# Fetching data from live API

<div style="position:relative;height:0;padding-bottom:56.25%"><iframe src="https://www.youtube.com/embed/g6-ZwZmRncs?ecver=2" width="640" height="360" frameborder="0" style="position:absolute;width:100%;height:100%;left:0" allowfullscreen></iframe></div>

Fetch is a built in interface to pull data or files into our application from somewhere on the internet.  Fetch uses promises to handle the asynchronous requests.  In its simplest form, fetch works like this:

```Javascript
fetch(<url>).then((rawResponse)=>{
  //return the part of the response we're interested in
  return rawResponse.json()
}).then(parsedResponse) => {
  //do what we need to with the parsedResponse, a JS object in this case
})
```

So, for our example, we need to make the request out to the NEO api, and handle the response, converting the response into an array of asteroids.

First, let's update our component's state with our API key and other properties we'll need to use in our API call.

```Javascript
class App extends Component {
  constructor(props){
    super(props)
    let today = new Date()
    this.state = {
      startDate:`${today.getFullYear()}-${today.getMonth()+1}-${today.getDate()}`,
      rawData: sampleNeo,
      asteroids: []
    }
}
```


Here is the completed ```componentWillMount``` method:

```Javascript
componentWillMount(){
  fetch("https://api.nasa.gov/neo/rest/v1/feed?"+`start_date=${this.state.startDate}&api_key=${<your apikey>}`).then((rawResponse)=>{
    // rawResponse.json() returns a promise that we pass along
    return rawResponse.json()
  }).then((parsedResponse) => {

    // when this promise resolves, we can work with our data
    let neoData = parsedResponse.near_earth_objects

    let newAsteroids = []
    Object.keys(neoData).forEach((date)=>{
      neoData[date].forEach((asteroid) =>{
        newAsteroids.push({
          id: asteroid.neo_reference_id,
          name: asteroid.name,
          date: asteroid.close_approach_data[0].close_approach_date,
          diameterMin: asteroid.estimated_diameter.feet.estimated_diameter_min.toFixed(0),
          diameterMax: asteroid.estimated_diameter.feet.estimated_diameter_max.toFixed(0),
          closestApproach: asteroid.close_approach_data[0].miss_distance.miles,
          velocity: parseFloat(asteroid.close_approach_data[0].relative_velocity.miles_per_hour).toFixed(0),
          distance: asteroid.close_approach_data[0].miss_distance.miles
        })
      })
    })

    // state is updated when promises are resolved
    this.setState({asteroids: newAsteroids})
  })
}
```

# Challenges
* Update the challenges you've worked on so far to pull live data from the API
