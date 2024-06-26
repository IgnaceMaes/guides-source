Currently, our app is using hard-coded data for our rental listings, defined in the `rentals` route handler.
As our application grows, we will want to persist our rental data on a server, and make it easier to do advanced operations on the data, such as querying.

Ember comes with a data management library called [Ember Data](https://github.com/emberjs/data) to help deal with persistent application data.

Ember Data requires you to define the structure of the data you wish to provide to your application by extending [`DS.Model`](https://api.emberjs.com/data/classes/DS.Model.html).

You can generate an Ember Data Model using Ember CLI.
We'll call our model `rental` and generate it as follows:

```bash
ember g model rental
```

This results in the creation of a model file and a test file:

```bash
installing model
  create app/models/rental.js
installing model-test
  create tests/unit/models/rental-test.js
```

When we open the model file, we can see a blank class extending [`DS.Model`](https://api.emberjs.com/data/classes/DS.Model.html):

```javascript {data-filename=app/models/rental.js}
import DS from 'ember-data';

export default DS.Model.extend({

});
```

Let's define the structure of a rental object using the same attributes for our rental that we [previously used](../model-hook/) in our hard-coded array of JavaScript objects -
_title_, _owner_, _city_, _property type_, _image_, _bedrooms_ and _description_.
Define attributes by giving them the result of the function [`DS.attr()`](https://api.emberjs.com/data/classes/DS.html#method_attr).
For more information on Ember Data Attributes, read the section called [Defining Attributes](../../models/defining-models/#toc_defining-attributes) in the guides.

```javascript {data-filename=app/models/rental.js}
import DS from 'ember-data';

export default DS.Model.extend({
  title: DS.attr(),
  owner: DS.attr(),
  city: DS.attr(),
  propertyType: DS.attr(),
  image: DS.attr(),
  bedrooms: DS.attr(),
  description: DS.attr()
});
```

We now have a model object that we can use for our Ember Data implementation.

### Updating the Model Hook

To use our new Ember Data Model object, we need to update the `model` function we [previously defined](../model-hook/) in our route handler.
Delete the hard-coded JavaScript Array, and replace it with the following call to the [Ember Data Store service](../../models/#toc_the-store-and-a-single-source-of-truth).
The [store service](https://api.emberjs.com/data/classes/DS.Store.html) is injected into all routes and components in Ember.
It is the main interface you use to interact with Ember Data.
In this case, call the [`findAll`](https://api.emberjs.com/data/classes/DS.Store.html#method_findAll) function on the store and provide it with the name of your newly created rental model class.

```javascript {data-filename=app/routes/rentals.js data-diff="+5,-6,-7,-8,-9,-10,-11,-12,-13,-14,-15,-16,-17,-18,-19,-20,-21,-22,-23,-24,-25,-26,-27,-28,-29,-30,-31,-32,-33"}
import Ember from 'ember';

export default Ember.Route.extend({
  model() {
    return this.get('store').findAll('rental');
    return [{
      id: 'grand-old-mansion',
      title: 'Grand Old Mansion',
      owner: 'Veruca Salt',
      city: 'San Francisco',
      propertyType: 'Estate',
      bedrooms: 15,
      image: 'https://upload.wikimedia.org/wikipedia/commons/c/cb/Crane_estate_(5).jpg',
      description: "This grand old mansion sits on over 100 acres of rolling hills and dense redwood forests."
    }, {
      id: 'urban-living',
      title: 'Urban Living',
      owner: 'Mike TV',
      city: 'Seattle',
      propertyType: 'Condo',
      bedrooms: 1,
      image: 'https://upload.wikimedia.org/wikipedia/commons/2/20/Seattle_-_Barnes_and_Bell_Buildings.jpg',
      description: "A commuters dream. This rental is within walking distance of 2 bus stops and the Metro."
    }, {
      id: 'downtown-charm',
      title: 'Downtown Charm',
      owner: 'Violet Beauregarde',
      city: 'Portland',
      propertyType: 'Apartment',
      bedrooms: 3,
      image: 'https://upload.wikimedia.org/wikipedia/commons/f/f7/Wheeldon_Apartment_Building_-_Portland_Oregon.jpg',
      description: "Convenience is at your doorstep with this charming downtown rental. Great restaurants and active night life are within a few feet."
    }];
  }
});
```

When we call `findAll`, Ember Data will attempt to fetch rentals from `/api/rentals`.
If you recall, in the section titled [Installing Addons](../installing-addons/) we set up an adapter to route data requests through `/api`.

You can read more about Ember Data in the [Models section](../../models/).

Since we have already set up Ember Mirage in our development environment, Mirage will return the data we requested without actually making a network request.

When we deploy our app to a production server,
we will likely want to replace Mirage with a remote server for Ember Data to communicate with for storing and retrieving persisted data.
A remote server will allow for data to be shared and updated across users.
