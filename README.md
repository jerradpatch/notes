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
