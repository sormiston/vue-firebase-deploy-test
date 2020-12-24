## VUE FORMS
### Max Schwazmueuller Complete Vue Section 12
<br/>

4. Sending a POST Request to Store Data
    * Re-route submitted surveys with a POST request to firebase in LearningSurvey component
    * Comment out all local app-level storage of submitted Surveys
    * App-level will no longer pass results props to User Experience component (comment that out too)
    * Test HTTP result.  Remember, firebase should be addressed at slug /surveys.json and the req body must be a JSON object.  Check if a surveys node was annexed to the firebase database tree.
    
5. GET Request & Transforming Response
   * push all results objs to an state array
   
7. Showing a Loading Message
    * add isLoading data prop and toggle it accordingly in the GET request method
    * use v-if else statements to show either loading message or the expected markdown
        
   
    axios
        .post('/surveys.json', {
          name: this.enteredName,
          rating: this.chosenRating,
        })
        .then((response) => console.log(response))
        .catch((error) => {
          if (error.response) {
            // The request was made and the server responded with a status code
            // that falls out of the range of 2xx
            
            console.log(error.response.data);
            console.log(error.response.status);
            console.log(error.response.headers);
          } else if (error.request) {
            // The request was made but no response was received
            // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
            // http.ClientRequest in node.js
            
            console.log(error.request);
            console.log('request error, above!');
          }
        });
