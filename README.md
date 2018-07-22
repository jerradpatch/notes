# notes
random collection of best programming practices learned from experience. Mostly related to javascript.


# MVC
## Models
- 1 Never add data to a DB model via a componenet / or add it in a well known sub-object. Why? Because a key could be added to the DB model who's value has a different purpose than used on the client. Perhaps its best to make the sub-object non-enumerible and fetched via a well function that was the object dbObject/model always remains serializible.
- 2 Every key in a model should have an api test, to make sure that the backend remains consistant.

- 2 Never use rxjs-subscriptions in a model that subscribe to data upon creation. For instance when trying to put together a destructured model. Why? Because the model maybe disposed of before the subscription's return value is ever needed.

- 3 When transforming data in an object, either do it in place or delete the original data key after the transformation is complete. B/c, adding keys to an object makes it difficult to know what data/keys can be relied upon as the object proceeds through stages of the application.

## Middle Layer
- 1 Return complete models to the cilents (assemeled destructured models). Why? 1) request efficentcy thus reduction in over all wait times, 2) reduction of model assembling logic on the front end.     

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
4) additional things: retying failed requests, caching reponses from resources without cache headers, validation


