Convert the code one file at a time.  If you code base is following conventions then you will generally have one Objective-C class per file, with maybe some enumerations and protocols hitching a ride as well.

In general the approach to conversion is:

1. Create a new `.swift` file with the same name as the Objective-C `.m` file (or the `.h` file if no `.m`)  Drag files around in Xcode so that the `.m`, `.h` and `.swift` files are next to each other.
2. Copy the Objective-C code from _both_ the `.h` and the `.m` file into the `.swift` file.  Consider it a framework for the conversion.
3. Convert the Objective-C code into Swift until you see no red dots or yellow triangles in Xcode.
4. Delete the old `.h` and `.m` files (they are recoverable from source code control, right?)
5. Compile and fix any introduced bridging problems.
6. Run tests and commit

Then move to the next file. Easy.
