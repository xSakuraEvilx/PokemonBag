xxHash.NET [![Build Status](https://travis-ci.org/wilhelmliao/xxHash.NET.svg?branch=master)](https://travis-ci.org/wilhelmliao/xxHash.NET)
==========
A .NET implementation of [xxHash](https://github.com/Cyan4973/xxHash). 

### Synopsis ###

#### xxHash API approach ####
*The following snippet demonstrates computing the `XXH32` hash value of the input string "test".*
```csharp
byte[] input = Encoder.ASCII.GetBytes("test");      // the data to be hashed
uint result = XXHash.XXH32(input);                  // compute the XXH32 hash value. => '1042293711'
                                                    // NOTE:
                                                    //   you can specified seed as the second parameter.
```

*The following snippet computes the `XXH32` hash value of the input file "test.doc".*
```csharp
Stream stream = File.OpenRead(@"C:\test.doc");      // the data to be hashed
XXHash.State32 state = XXHash.CreateState32();      // create and initialize a xxH states instance.
                                                    // NOTE:
                                                    //   xxHash require a xxH state object for keeping
                                                    //   data, seed, and vectors.
                                                    
UpdateState32(state, stream);                       // puts the file stream into specified xxH state.

uint result = DigestState32(state);                 // compute the XXH32 hash value.
```

###### Supported xxHash APIs: ######

| original xxHash API name | XXH32             | XXH64             |
|--------------------------|-------------------|-------------------|
| XXH*nn*()                | XXH32()           | XXH64()           |
| XXH*nn*_state_t          | State32           | State64           |
| XXH*nn*_createState()    | CreateState32()   | CreateState64()   |
| XXH*nn*_freeState()      | *Not implemented* | *Not implemented* |
| XXH*nn*_reset()          | ResetState32()    | ResetState64()    |
| XXH*nn*_update()         | UpdateState32()   | UpdateState64()   |
| XXH*nn*_digest()         | DigestState32()   | DigestState64()   |

#### HashAlgorithm approach ####
In addition, the assembly also provides `XXHash32` and `XXHash64` the two implementation classes of *System.Security.Cryptography.HashAlgorithm*.

*The following snippets demonstrate computing the `XXH32` hash value with HashAlgorithm approach.*
```csharp
byte[] input = Encoder.ASCII.GetBytes("test");         // the data to be hashed.
using (HashAlgorithm xxhash = XXHash32.Create())
{
  byte[] result = xxhash.ComputeHash(input);           // compute the hash.
}
```
-- *or* --
```csharp
byte[] input = Encoder.ASCII.GetBytes("test");         // the data to be hashed
using (HashAlgorithm xxhash = XXHash32.Create())
{
  xxhash.TransformFinalBlock(input, 0, input.Length);
  byte[] result = xxhash.Hash;                         // retrieves the hash value.
}
```
**NOTE:** XXH64 is also supported: you can use `xxHash64` class instead of `xxHash32`.



#### Versioning ####
[v1.0](https://github.com/wilhelmliao/xxHash.NET/releases/tag/v1.0) (equal to [xxHash r42](https://github.com/Cyan4973/xxHash/releases/tag/r42))

-----------

#### Copyright ####
Copyright (c) 2015 Wilhelm Liao. See [LICENSE](https://github.com/wilhelmliao/xxHash.NET/blob/master/LICENSE) for further details.
