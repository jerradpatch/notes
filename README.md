# notes
random collection of best programming practices learned from experience. Mostly related to javascript.


# MVC
## Models
- 1 Never add data to a DB model via a componenet / or add it in a well known sub-object. Why? Because a key could be added to the DB model who's value has a different purpose than used on the client. Perhaps its best to make the sub-object non-enumerible and fetched via a well function that was the object dbObject/model always remains serializible.

- 2 Never use rxjs-subscriptions in a model that subscribe to data upon creation. For instance when trying to put together a destructured model. Why? Because the model maybe disposed of before the subscription's return value is ever needed.

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

## Angular ^2.x.x
- When writing controller functions make a function that returns a value and then a function that emits based on the returned value. B/c somtimes it is good to emit a value and other times the controller function maybe used directly via @ViewChild/ren
- Instead of using "@input set function" use the changes life cycle: 1) frees the thread to process the remaining change cycle, 2) if an output is needed to be emitted, it can be emitted without need to force a change detection event.

## RxJs 
- Dont use anonamous functions when chaining stream manipulations: 1) add to ability to debug and understand preformance issues, 2) adds to the readibility of the code.

