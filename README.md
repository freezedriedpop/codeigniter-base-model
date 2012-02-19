codeigniter-base-model
=====================================

My CodeIgniter Base Model is an extended CI_Model class to use in your CodeIgniter applications. It provides a full CRUD base to make developing database interactions easier and quicker. It also includes a bunch of other cool stuff, including before and after create callbacks, validation and a some table name guessing.

Synopsis
--------

```php
class Post_model extends MY_Model { }

$this->load->model('post_model', 'post');

$this->post->get_all();

$this->post->get(1);
$this->post->get_by('title', 'Pigs CAN Fly!');
$this->post->get_many_by('status', 'open');

$this->post->insert(array(
    'status' => 'open',
    'title' => "I'm too sexy for my shirt"
));

$this->post->update(1, array( 'status' => 'closed' ));

$this->post->delete(1);
```

Usage
-----

Drag the MY\_Model.php file into your _application/core_ folder. CodeIgniter will load and initialise this class automatically for you. Extend all your model classes from MY_Model and all the functionality will be baked into your models automatically.

Naming Conventions
------------------

This class will try to guess the name of the table to use, by guessing the plural of the class name. If the table name isn't the plural and you need to set it to something else, just declare the _$\_table_ instance variable and set it to the table name. Some of the CRUD functions also assume that your primary key ID column is called _'id'_. You can overwrite this functionality by setting the _$primary\_key_ instance variable.

Callbacks
---------

There are many times when you'll need to alter your model data before it's inserted or returned. This could be adding timestamps, pulling in relationships or deleting dependent rows. The MVC pattern states that these sorts of operations need to go in the model. In order to facilitate this, **MY_Model** contains a series of callbacks -- methods that will be called at certain points.

The full list of callbacks are as follows:

* $before_create
* $after_create
* $before_update
* $after_update
* $before_get
* $after_get
* $before_delete
* $after_delete

These are instance variables usually defined at the class level. They are arrays of methods on this class to be called at certain points. An example:

```php
class Book_model extends MY_Model
{
    public $before_create = array( 'timestamps' );
    
    protected function timestamps($book)
    {
        $book['created_at'] = $book['updated_at'] = date('Y-m-d H:i:s');
    }
}
```

Validation
----------

This class also includes some excellent validation support. This uses the built-in Form Validation library and provides a wrapper around it to make validation automatic on insert. To enable, set the *$validate* instance variable to the rules array that you would pass into `$this->form_validation->set_rules()`. To find out more about the rules array, please [view the library's documentation](http://codeigniter.com/user_guide/libraries/form_validation.html#validationrulesasarray).

Then, for each call to `insert()`, the data passed through will be validated according to the *$validate* rules array. **Unlike the CodeIgniter validation library, this won't validate the POST data, rather, it validates the data passed directly through.**

If for some reason you'd like to skip the validation, you can call `skip_validation()` before the call to `insert()` and validation won't be performed on the data for that single call.

Joins
--------

This can now be used to make joining tables together easier with predefined clauses

This is what you used to have to do

```php
$this->db->join('user_profiles', 'users.user_id = user_profiles.user_profile_id', 'inner');
$this->post->get_all();
```
Now all you have to do is this

```php
$this->db->join('user_profiles')->get_all();
```

To get this to work you'll need to set the relationships at the top of the class

```php
class Users_model extends MY_Model {
    public $relationships = array(
		'user_profiles'	=>	array(
			'type'		=>	'inner',
			'condition'	=>	'users.user_id = user_profiles.user_profile_id'
		)
	);
}
```

Other Documentation
-------------------

* Jeff Madsen has written an excellent tutorial about the basics (and triggered me updating the documentation here). [Read it now, you lovely people.](http://www.codebyjeff.com/blog/2012/01/using-jamie-rumbelows-my_model)

Contributors
------------

Thanks to:
    
* [Phil Sturgeon](http://philsturgeon.co.uk)
* [Dan Horrigan](http://danhorrigan.com)
* [Adam Jackett](http://darkhousemedia.com)
    
...who have all contributed a great amount of code and ideas to this MY_Model.

Joins added by * [Rob Brain](http://twitter.com/freezedriedpop) based on original work by * [Jamie Rumbelow](jamierumbelow.net)