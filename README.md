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