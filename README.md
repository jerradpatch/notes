# notes
random collection of best programming practices learned from experience. Mostly related to javascript.



# MVC
## Models
- 1 Never add data to a DB model via a componenet / or add it in a well known sub-object. Why? Because a key could be added to the DB model who's value has a different purpose than used on the client. Perhaps its best to make the sub-object non-enumerible and fetched via a well function that was the object dbObject/model always remains serializible.
- 2 Every key in a model should have an api test, to make sure that the backend remains consistant.

- 2 Never use rxjs-subscriptions in a model that subscribe to data upon creation. For instance when trying to put together a destructured model. Why? Because the model maybe disposed of before the subscription's return value is ever needed.

- 3 When transforming data in an object, either do it in place or delete the original data key after the transformation is complete. B/c, adding keys to an object makes it difficult to know what data/keys can be relied upon as the object proceeds through stages of the application.
### concept: Many Models One Wrapper, WIP
- A database may hold may different models with many different keys and values. However, in the client side models are applied to components, and components are reusible pieces of code that are used in multiple places in an application: styled lists, cards in a list box. Many components are tied to a specific api and thus model, however each component can have many common things with other components with the model being the only difference. If there was one model wrapper that normalized all the keys for the components use many components can be reused or expanded to encompass more functionality (gettes / seters). In addition, all key transformation logic, that would normally happen in the component, could be isolated to the model. Isolating transformation logic to the model reduces transformation logic across the entire application. Thus, every component has an interface, that is applied in the wrapper of every model that comes back from the api.
#### Application:
```typescript
let modA: {title: string, occurrence: number, language: string, imageUrl: string, videoUrl: string}
let modB: {place: string, location: {long: number, lat: number}, imageUrls: string[]}

class OneModelExample {
  //pseudo code,
  // **note the data from the database in not modified.
  _data = {
    title: string, occurrence: number, language: string, videoUrl: string,
    place: string, location: {long: number, lat: number}, imageUrls: string[]
  }
  
  get title(): string {
    return this._data.title || this._data.place;
  }
  
  get imageUrls(): any[] {
    return this._data.imageUrl && [this._data.imageUrl] || this._data.imageUrls || [];
  }
  
  get videoUrls(): any[] {
    return this._data.videoUrl && [this._data.videoUrl] || [];
  }
  
  get occurrence(): number {
    return this._data.occurrence;
  }
}

//any component interface
interface ComponentA {
  title: string,
  imageUrls: string[]
}
interface ComponentB {
  title: string,
  imageUrls: string[],
  videoUrls: string[]
}[]
```
In the example above, all transformation logic has been located to a single place for all models from the database. All components have a normalized interface that can be expanded by using more normalized keys. Note, no transformations were preformed to modify the original data, because this may change the actual meaning of the key in the database. For instance if we modified a 'seriesTitle' to just 'title' we have lost the meaning and the value can no longer be identified as a 'seriesTitle'.

## Middle Layer
- 1 Return complete models to the cilents (assemeled destructured models). Why? 1) request efficentcy thus reduction in over all wait times, 2) reduction of model assembling logic on the front end.     
### Architecture
#### Multiplue Databases / Same Models:
  1) models in every collection should have a key or key combination that uniquly identifies that model, egardless of what database it is in. This reduces transformation code and starts establishing a standard model key set.
  2) models should inherit distinction keys from parent models. Ex: a series has episodes, what makes a series distinct is 'seriesTitle:string, season:number, seriesType: string' = "'this dog', 2, 'series'". A episode in that series will look like 'seriesTitle:string, season:number, seriesType: string, episodeTitle: string, occurrence: number' 
  = "'this dog', 2, 'series', 'the dog goes to market', 1". While, duplication will occur the it is unlikly that keys used as an id for a model will ever change as long as the model exists. The benefit is that an member of the herarchy can be used to find other members without as second query and without any transformations. For example, if the series was desired and the current episode was given, the series could be found by doing a search on the series collection with the episode model, given that we only use the series distinction keys from the episode to search for the series. Inversly, the series could be used on the episode collection to find all episodes from that series. This can be taken further if each episode had image and video references (multiplue languages). We could use a series model to find all episodes models from the episode collection or all videos in every episode and all languages by using the series model in a search on the videos collection; or use a video model to find the series. All of this can be done without a single reference by simple establishing a common set of distinct keys for a given model across all databases.
  
## Typescript
- Always destructure all incoming objects to to a component to the point that the component uses that input. Ie, dont reference a type imported from another class as this class does not change its usage of the type relative to the other class. Importing types leads to less reusibility of code and causes refactoring as the object changes, or causes the imported object to be purpose overloaded.

### Functions
- Always put varibles that default to falsy at the end of the function, so that they maybe undefined/not-given when desired to be false.
- Keep functions that have side-efects / modify external space to a minimum: increases code reusibility.
- Always use temp-varibles to store the results of function calls in parent functions, makes debugging easier.
- Prefix Objects with the with conceptual usage type: MEpisode = Model Episode, IMoney = interface money, SData = service data; this makes it easily understood what the use case of the object is in code without having to look it up (read it's code). M=model, C=component, I=interface, S=service.
- boolean values always default to false.
- IF statements - should always have an 'else' clause when setting parameters outside the scope (side effects), regardless of domain knowledge.
- conditional statements and values - try to not write conditional statements that are based on an objects value, instead write code that will work regardless of the value. This will lead to reusible code and less work overall. It will also lead to code that works even when the values are added/removed/changed. If code must depend on the value of an object then display a notification when the value doesn't fall within the expected values.

### JSON
- All boolean values should logically default to false, key names should reflect this. So, a key doesn't need to be defined to be set correctly.

## Angular ^2.x.x
- When writing controller functions make a function that returns a value and then a function that emits based on the returned value. B/c somtimes it is good to emit a value and other times the controller function maybe used directly via @ViewChild/ren
- Instead of using "@input set function" use the changes life cycle: 1) frees the thread to process the remaining change cycle, 2) if an output is needed to be emitted, it can be emitted without need to force a change detection event.

## RxJs 
- Dont use anonamous functions when chaining stream manipulations: 1) add to ability to debug and understand preformance issues, 2) adds to the readibility of the code.

## Rest Endpoints
### Client Considerations:
1) timeout is always needed
2) what to do when failure occurs
3) consider it was a 200 yet was a failure
4) additional things: throttleing requests, retying failed requests, caching reponses from resources without cache headers, validation, normalizing remote models to local models key/values, page objects to pull data from pages, tests to make sure page objects continue to pull expected data.

## Scraping data or data mining a website - WIP
- all considerations of a rest endpoint need to be considered above
- Consider a scrapped website to be a database where models are being returned based on a query. For example, filling out an episode or series model.
- distinction keys can be used to make up the basis for queries in the database. In an episode for example, 
```typescript
let episode: {
  title: string
  season: number,
  seasonType: string,
  episodeOccurrence: number,
  subtitles: string,
  audio: string
}
```
the above keys could be used to make a query to find all seasons = 2, or simply every episode in a given season. So, using this architecture a scraping flow would look like
1) get a listing of all models filled in the database with only the distinction key's values filled, or create a url using the query distinction keys.
2) filter each model whoes distinctions keys do not match the query distinction keys
3) go to the database and fill out each non-filter model with remaining non-distinction key data: episode title, summary...

#### Architecture of scrapper - WIP
1) each page has its own file
2) in the file, each page has a page object, each page has 

