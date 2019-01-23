https://player.vimeo.com/video/226597203

# API Data
Working with third-party data requires us to parse the data we receive and transform it into something useful for the application.  Depending on how complex the API is, this can be somewhat challenging, and takes some careful consideration on our part to do efficiently.  In the NEO API response, the asteroid data comes in a different format than how we want to use it in our HTML table, so we have to map it to a structure that makes sense for us and our app.

This is where having the saved file to use as a reference as we're developing provides a lot of value.  Instead of making a call across the web to the API, we can just load the file, experiment, and work with the data until we get exactly what we want.  Not only does this save us a lot of time, it reduces strain on NASA's servers, which they will undoubtedly appreciate.

## Mapping API Data

Consider the following structure of the NEO API response:

```JSON
{
  "links" : {
    "next" : "https://api.nasa.gov/neo/rest/v1/feed?start_date=2017-08-07&end_date=2017-08-14&detailed=false&api_key=NT8V130OXGjNIRbuEbvygKwFipek7WxYXI8nn1o9",
    "prev" : "https://api.nasa.gov/neo/rest/v1/feed?start_date=2017-07-24&end_date=2017-07-31&detailed=false&api_key=NT8V130OXGjNIRbuEbvygKwFipek7WxYXI8nn1o9",
    "self" : "https://api.nasa.gov/neo/rest/v1/feed?start_date=2017-07-31&end_date=2017-08-07&detailed=false&api_key=NT8V130OXGjNIRbuEbvygKwFipek7WxYXI8nn1o9"
  },
  "element_count" : 49,
  "near_earth_objects" : {
    "2017-08-05" : [ {
      "links" : {
        "self" : "https://api.nasa.gov/neo/rest/v1/neo/3102785?api_key=NT8V130OXGjNIRbuEbvygKwFipek7WxYXI8nn1o9"
      },
      "neo_reference_id" : "3102785",
      "name" : "(2002 BG25)",
      "nasa_jpl_url" : "http://ssd.jpl.nasa.gov/sbdb.cgi?sstr=3102785",
      "absolute_magnitude_h" : 20.9,
      "estimated_diameter" : {
        "kilometers" : {
          "estimated_diameter_min" : 0.1756123185,
          "estimated_diameter_max" : 0.3926810818
        },
        "meters" : {
          "estimated_diameter_min" : 175.6123184804,
          "estimated_diameter_max" : 392.6810818086
        },
        "miles" : {
          "estimated_diameter_min" : 0.1091204019,
          "estimated_diameter_max" : 0.2440006365
        },
        "feet" : {
          "estimated_diameter_min" : 576.1559189633,
          "estimated_diameter_max" : 1288.3238004408
        }
      },
      "is_potentially_hazardous_asteroid" : false,
      "close_approach_data" : [ {
        "close_approach_date" : "2017-08-05",
        "epoch_date_close_approach" : 1501916400000,
        "relative_velocity" : {
          "kilometers_per_second" : "24.5177734759",
          "kilometers_per_hour" : "88263.9845133905",
          "miles_per_hour" : "54843.8074883342"
        },
        "miss_distance" : {
          "astronomical" : "0.3164080673",
          "lunar" : "123.0827407837",
          "kilometers" : "47333972",
          "miles" : "29411968"
        },
        "orbiting_body" : "Earth"
      } ]
    }, {
      "links" : {
        "self" : "https://api.nasa.gov/neo/rest/v1/neo/3276601?api_key=NT8V130OXGjNIRbuEbvygKwFipek7WxYXI8nn1o9"
      },
      "neo_reference_id" : "3276601",
      "name" : "(2005 GB120)",
      "nasa_jpl_url" : "http://ssd.jpl.nasa.gov/sbdb.cgi?sstr=3276601",
      "absolute_magnitude_h" : 20.4,
      "estimated_diameter" : {
        "kilometers" : {
          "estimated_diameter_min" : 0.2210828104,
          "estimated_diameter_max" : 0.4943561926
        },
        "meters" : {
          "estimated_diameter_min" : 221.0828103591,
          "estimated_diameter_max" : 494.3561926196
        },
        "miles" : {
          "estimated_diameter_min" : 0.137374447,
          "estimated_diameter_max" : 0.3071786018
        },
        "feet" : {
          "estimated_diameter_min" : 725.3373275385,
          "estimated_diameter_max" : 1621.9035709942
        }
      },
      "is_potentially_hazardous_asteroid" : false,
      "close_approach_data" : [ {
        "close_approach_date" : "2017-08-05",
        "epoch_date_close_approach" : 1501916400000,
        "relative_velocity" : {
          "kilometers_per_second" : "8.6908491074",
          "kilometers_per_hour" : "31287.056786697",
          "miles_per_hour" : "19440.5603683785"
        },
        "miss_distance" : {
          "astronomical" : "0.3528578146",
          "lunar" : "137.2616882324",
          "kilometers" : "52786780",
          "miles" : "32800184"
        },
        "orbiting_body" : "Earth"
      } ]
    },
    .....
  }
}
```

In this response, we have 3 top level attributes: links, element_count, near_earth_objects.  The section we're interested in is the 'near_earth_objects' part, so the first thing we can do is assign that part to a variable, and ignore the rest.  Once we have hold of what we want, we can iterate over it and map the data into a new, more useful, data structure:

```Javascript
componentWillMount(){
    // Get hold of the part of the response we are interested in
    let neoData = this.state.rawData.near_earth_objects

    // Instantiate a new array to hold our mapped data
    let newAsteroids = []

    // To iterate of attributes of a JS Object, we call: Object.keys()
    Object.keys(neoData).forEach((date)=>{

      // Object.keys returns the name of the attribute, so we need to access that attribute
      // on our data structure, and loop over each asteroid it contains
      neoData[date].forEach((asteroid) =>{

        // Now that we have the asteroid, we can add it to our newAsteroids array
        newAsteroids.push({
          id: asteroid.neo_reference_id,
          name: asteroid.name,
          date: asteroid.close_approach_data[0].close_approach_date,

          // Calling '.toFixed(0)' on a float cuts off everything behind the decimal point.
          // This formats the information for a good user experience.
          diameterMin: asteroid.estimated_diameter.feet.estimated_diameter_min.toFixed(0),
          diameterMax: asteroid.estimated_diameter.feet.estimated_diameter_max.toFixed(0),
          closestApproach: asteroid.close_approach_data[0].miss_distance.miles,
          velocity: parseFloat(asteroid.close_approach_data[0].relative_velocity.miles_per_hour).toFixed(0),
          distance: asteroid.close_approach_data[0].miss_distance.miles
        })
      })
    })

    // Finally, now that we have collected all the asteroids, we can assign them to state
    // so that we can use them later on in the render function
    this.setState({asteroids: newAsteroids})
}
```

*Notes:
* Here we are setting the value of state's asteroid property. If this.state.asteroid is coming up undefined, check to see that state contains an asteroid property.

## Challenges
* create a React app (or reuse the app from the previous challenge) and import a cached API response object from the NEO Api
* create a new React app, and curl the following API request into a sample data file: https://api.nasa.gov/neo/rest/v1/neo/2465633?api_key=<your api key>
* Import the data file, and add the following attributes to the component state:
  * reference_id
  * name
  * diameter_in_feet
  * closest_approach_dates:  (this is a list of all the known close approaches for this asteroid to earth)
