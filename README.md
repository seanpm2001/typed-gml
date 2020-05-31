# typed-gml
Typed GML is an attempt to add typed functions, constructors and methods in the new Game Maker Studio 2.3 beta.

Supported type checks:
* check if a struct came from a specific constructor
* instance object_index checks
* all is_ functions (is_real, is_array, is_string etc)

An example usage:
```JavaScript
globalvar Entity;
Entity = function(_name, _instance) constructor {
	assert_types(is_string(_name));
	
	name = _name;
	instance = _instance;
}

function SimpleStorer(_instance) constructor {
	assert_types(is_instance_of(_instance, obj_simple));
	
	instance = _instance
}

function Vec2(_x, _y) constructor {
	assert_types(is_real(_x), is_real(_y));
	
	x = _x;
	y = _y;
	
	static Add = function( _other ){
		assert_types([_other, self]);
		
		x += _other.x;
		y += _other.y;
	}
}

globalvar Struct;
Struct = function(_entity, _vector) constructor {
	assert_types([_entity, Entity], [_vector, Vec2]);
	
	entity = _entity;
	vector = _vector;
	
	static get_entity = function() {
		return entity;
	}
	
	static set_entity = function(_entity) {
		assert_types([_entity, Entity]);
		entity = _entity;
	}
}

// Successful
var _struct = new Struct(new Entity("name"), new Vec2(0.0, 0.0));

// Will throw a type error
var _struct = new Struct(new Entity("name"), new Entity("name"));
```

