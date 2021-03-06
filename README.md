## Golang | Protocol Buffers
My personal notes and sample codes.

## Author
Aditya Hajare ([Linkedin](https://in.linkedin.com/in/aditya-hajare)).

## Current Status
WIP (Work In Progress)!

## License
Open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT).

-----------

## Advantages of Protocol Buffers:
- Data is fully typed.
- Data is compressed automatically (less CPU usage).
- `Schema` (defined using `.proto` file) is needed to generate code and read the data.
- Documentation can be embedded in the `Schema`.
- Data can be read across any language (C#, Java, Go, Python, JavaScript, etc..).
- `Schema` can evolve over time in a safe manner (`Schema` evolution).
- 3-10x smaller, 20-100x faster than XML.
- Code is generated for us automatically.

## Disadvantages of Protocol Buffers:
- `Protobuf` support for some languages might be lacking (but the main ones are fine).
- Can't open the serialized data with a text editor (because it's compressed an serialized).

-----------

## Tags:
- In protocol buffers, field names are not important! But when programming, fields/field names are important.
- For `protobuf`, the important element is the `tag`.
- Smallest Tag number we can have is `1`.
- Largest Tag number we can have is `2²⁹-1` i.e. `536,870,911`.
- We cannot use numbers between `19000` to `19999`. These are reserved by Google for special use.
- Tags numbered from `1` to `15` use `1 byte` in space, so use them for frequently populated fields.
- For fields those are less populated, use Tag numbers from `16` to `2024`. They use `2 bytes` in space.

-----------

## Repeated Fields:
- To make a `list` or an `array`, we can use a concept of `Repeated Fields`.
- The list can take any number (0 or more) of elements we want.
- The opposite of `repeated` is `singular` (We don't write it).

-----------

## Enums:
- If we know all the values a field can take in advance, we can leverage an `Enum` type.
- **The first value of an `Enum` is the Default value.**
- `Enum` must start by the tag `0` (which is the default value).

-----------

## Generating Golang Code Using Protoc:
- Following command is used to generate Golang code:
```sh
# Browse to the directory
cd ~/work/Golang/golang_protocol_buffers/02-Protoc-To-Generate-Golang-Code

# protoc: Compiler for protocol buffers
# -I: specifies source directory where protocol buffer files are resided
# --go_out: specifies output directory
# At the end specify path to protocol buffer file(s)
protoc -I=proto --go_out=go proto/*.proto
```

-----------

## Rules For Updating Protocol
- Don't change the numeric tags for any existing fields.
- We can add new fields, and old code will just ignore them.
- Likewise, if old/new code reads unknown data, the defaults will take place.
- Fields can be removed as long as the tag number is not used again in our updated message type. We may want to field instead, perhaps adding the prefix `OBSOLUTE_`, or make the tag `reserved` so that future uses of our `.proto` can't accidentially reuse the number.
- For changing data types (e.g. `int32` to `int643`) we must refer to the documentations.
- **When removing a field, we must always `reserve` the tag and the field name!** This prevents `tag` and field name to be re-used. For e.g.
```proto
    // Original Message
    message Message {
        int32 id = 1;
        string first_name = 2;
    }

    // Lets remove field "first_name"
    message Message {
        reserved 2;            // Reserved tag number
        reserved "first_name"; // Reserved field name
        int32 id = 1;
    }
```

-----------

## Reserved Keywords
- We can reserve `TAGS` and `FIELD NAMES`.
- We can't mix `TAGS` and `FIELD NAMES` in the same `reserved` statement. For e.g.
```proto
    // Correct way to use "reserved" keyword:
    message Message {
        reserved 2, 4, 15, 20 to 30;
        reserved "first_name", "last_name";
    }
```
- **Do not EVER remove any RESERVED tags!**

-----------

## oneof | Advanced Types:
- We can use `oneof` to tell protocol buffers that only one field can have a value set to it. For e.g.:
```proto
    message HelloAditya {
        int32 id = 1;

        oneof some_name_field {
            // In "some_name_field" either "name" or
            // "first_name" field will have value set to it.
            string name = 2;
            string first_name = 3;
        }
    }
```

-----------

## Maps | Advanced Types:
- Maps can be used to define scaler message types. It's like a `dictionary` in python or `structs` in go:
```proto
    message Message {
        int32 id = 1;
        map<string, Result> results = 2;
    }
```
- Map fields cannot be repeated.
- THere's no ordering for map (its `key => value` store).

-----------

## Timestamp (Well Known Types) | Advanced Types:
- Protocol Buffers contains a set of `Well Known Types`. For e.g. advanced types known to all programming languages.
- Full list of `Well Known Types`: [https://developers.google.com/protocol-buffers/docs/reference/google.protobuf](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf)
- One of the types is `Timestamp` - fields are `seconds` and `nanoseconds` (UTC).
- Don't forget to use the `import` statement.
- For e.g.
```proto
     syntax = "proto3";
     import "google/protobuf/timestamp.proto";

     message Sample {
         google.protobuf.Timestamp my_timestamp = 1;
     }
```

-----------

## Duration (Well Known Types) | Advanced Types:
- `Duration` is yet another `Well Known Type`.
- It represents the time span between 2 timestamps.
- Just like `Timestamp`, it contains `seconds` and `nanoseconds`.
- For e.g.
```proto
    syntax = "proto3";

    import "google/protobuf/timestamp.proto";
    import "google/protobuf/duration.proto";
        message Sample {
         google.protobuf.Timestamp my_timestamp = 1;
         google.protobuf.Duration validity = 2;
     }
```