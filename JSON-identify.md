Specification
-------------

## Abstract
JSON-identify is a specification that describes how data in JSON format can be represented to be identifiable. This offers the possibility to locate data based on the identification being defined in this specification.

## 1.	Introduction
The JSON-identify format makes it possible to uniquely identify and locate a JavaScript Object Notation (JSON) [RFC7159](https://tools.ietf.org/html/rfc7159) formatted object. Any JSON formatted object MAY contain the definitions to identify the object. Not having the defined identifiers does not render the object invalid. It would just not be identifiable. Identifying objects also allows for the use of Uniform Resource Identifiers (URIs) [RFC3986](https://tools.ietf.org/html/rfc3986).

### 1.1.	Conventions Used in This Document
The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC2119](https://tools.ietf.org/html/rfc2119).

The grammatical rules in this document are to be interpreted as described in [RFC5234](https://tools.ietf.org/html/rfc5234).

## 2.	Document Structure
A JSON-identify document is a JSON [RFC7159](https://tools.ietf.org/html/rfc7159) document that represents an object by itself.

### 2.1.    Idenitifiers
The object MUST contain at least one of three possible identifiers. The object MAY include all possible identifiers, which allows for the location of the object in multiple ways. Additional properties of the document MAY be added in optional defined data objects. The identifiers and the data objects defined in this document all start with an underscore "_" to easily recognize these.

The following is an example of a JSON-identify document containing all three possible identifiers:

```JSON-identify Object
{
	"_id": 5378,
	"_name": "john-doe",
	"_path": "person/john-doe",
	"_data": {
		"firstname": "John",
		"lastname": "Doe"
	},
	"_metadata": {
		"ssn": "123456789"
	},
	"comments": "Person is visually impaired."
}
``` 

In this example, the identifiers are `_id`, `_name`, and `_path`. The OPTIONAL data objects are `_data`, `_metadata`, and `_arguments`. The comments are not part of this specification.

Similarly, the same object can be identified just by `_id`, thus limiting the methods to identify the object:

```JSON-identify Object
{
	"_id": 5378,
	"_data": {
		"firstname": "John",
		"lastname": "Doe"
	},
	"_metadata": {
		"ssn": "123456789"
	},
}
``` 

Or by only using `_name`:

```JSON-identify Object
{
	"_name": "john-doe",
	"_data": {
		"firstname": "John",
		"lastname": "Doe"
	}
}
``` 

In any case, each object MUST have at least one of the identifiers to be considered a JSON-identify object.

### 2.1.    Identifiers
The following identifiers are defined:

|   Identifier  |   Type                |   Purpose             |
|   ----------  |   ----                |   -------             |
|   _id         |   int                 |   Numeric identifier  |
|   _name       |   unreserved string   |   Readable identifier	|
|   _path       |   JSON pointer        |   Path as described	|

The type "unreserved string" refers to a string that only uses 'unreserved' characters, as specified in [RFC3986 section 2.3](https://tools.ietf.org/html/rfc3986#section-2.3). This allows for easy application in URIs. The "unreserved string" can be represented as follows:

unreserved string	ALPHA / DIGIT / "-" / "." / "_" / "~"

The type JSON pointer refers to a string that complies with the specifications as described in [RFC6901](https://tools.ietf.org/html/rfc6901).

### 2.2.    Optional Data Objects
A few optional data objects are defined

|   Identifier  |   Type                |   Purpose                                                             			|
|   ----------  |   ----                |   -------                                                             			|
|   _data       |   object *or* array   |   Object or array of objects containing the properties of that specific object	|
|   _metadata   |   object   			|   Object containing metadata of the object                               			|

The `_data` data object is defined additionally to distinguish between the identifiers and the actual data. When coding for JSON-identify, access to these properties can be easily translated to simplify the access. For example, using JavaScript, the property `firstname` in the previous example, COULD be accessed either through `person._data['firstname']` or `person['firstname']`. In the latter form, the properties would be loaded at a higher level.

Note:	In this case, the properties MAY NOT be using the property names as defined in this document. It is RECOMMENDED to avoid using names starting with underscore "_" for these properties.

The `_metadata` data object makes it possible to separate metadata from the actual object data. These SHOULD NOT be transcribed to another access format. This data object can contain for example, the creation date, the author, or, as in the earlier example, other references.

Both optional data objects COULD themselves be JSON-identify objects, thus allowing backward references, or simpler integration with data sources.

A simple example is shown below:

```JSON-identify Object
{
	"_name": "languagesSpoken",
	"_data": [
		{
			"_name": "en",
			"_data": {
				"description": "English"
			}
		},
		{
			"_name": "fr",
			"_data": {
				"description": "French"
			}
		}
	]
}
```

