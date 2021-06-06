# FieldConverter

Convert anything to anything\*

- : if a generic conversion mechanism for given source and given destination types is implemented

For instance,

## Convert basic types to string:

```
TEST(TestFixture, BasicTypes_To_String ) {
    bool                b   = true;
    char                ch  = 'c';
    unsigned char       u8  = 'C';
    int16_t             i16 = 10;
    uint16_t            u16 = 12;
    int32_t             i32 = 12;
    uint32_t            u32 = 12;
    int64_t             i64 = 12;
    uint64_t            u64 = 12;
    float               f   = 12;
    double              d   = 12;
    EXPECT_EQ( "true"       , ytl::convert_to<std::string>( b   ) );
    EXPECT_EQ( "c"          , ytl::convert_to<std::string>( ch  ) );
    EXPECT_EQ( "C"          , ytl::convert_to<std::string>( u8  ) );
    EXPECT_EQ( "10"         , ytl::convert_to<std::string>( i16 ) );
    EXPECT_EQ( "12"         , ytl::convert_to<std::string>( u16 ) );
    EXPECT_EQ( "12"         , ytl::convert_to<std::string>( i32 ) );
    EXPECT_EQ( "12"         , ytl::convert_to<std::string>( u32 ) );
    EXPECT_EQ( "12"         , ytl::convert_to<std::string>( i64 ) );
    EXPECT_EQ( "12"         , ytl::convert_to<std::string>( u64 ) );
    EXPECT_EQ( "12.000000"  , ytl::convert_to<std::string>( f   ) );
    EXPECT_EQ( "12.000000"  , ytl::convert_to<std::string>( d   ) );
}
```

## Strings to Basic types:

```
TEST(TestFixture, RvalueString_To_BasicTypes ) {
    createFieldAndCheckResult< std::string, float >( "123.456", 123.456f );
    createFieldAndCheckResult< std::string, int32_t >( "123.456", 123l );
    createFieldAndCheckResult< std::string, int64_t >( "123.456", 123ll );
    createFieldAndCheckResult< std::string, uint32_t >( "123.456", 123ul );
    createFieldAndCheckResult< std::string, uint64_t >( "123.456", 123ull );
    createFieldAndCheckResult< std::string, double >( "123.456", 123.456 );
}
```

## Instantly convert complex structured data:

```
    struct Data {
        std::array<char, 2> mm;
        char ch;
        uint8_t ui8;
        uint16_t ui16;
        uint32_t ui32;
        uint64_t ui64;
        int32_t  i32;
        bool flag;
    };
```

Then simply define a `Field<V,T>` where,

- V is the source data type
- T is the target data type

for above structure it would be like:

```
    struct FieldInData {
        Field< std::array<char, 2>, std::string > arr;
        Field< char     , std::string > ch;
        Field< uint8_t  , std::string > ui8;
        Field< uint16_t , std::string > ui16;
        Field< uint32_t , std::string > ui32;
        Field< uint64_t , std::string > ui64;
        Field< int32_t  , std::string > i32;
        Field< bool     , std::string > flag;
    };
```

Then following will pass:

```
    EXPECT_EQ( "ab", data.arr.yield() );
    EXPECT_EQ( "c", data.ch.yield() );
    EXPECT_EQ( "16", data.ui16.yield() );
    EXPECT_EQ( "32", data.ui32.yield() );
    EXPECT_EQ( "64", data.ui64.yield() );
    EXPECT_EQ( "32", data.i32.yield() );
    EXPECT_EQ( "false", data.flag.yield() );
}
```

## One more date related example:

```
TEST(TestFixture, CustomDateString_To_Numbers) {
    struct Data {
        std::array<char, 2> dd;
        char seperator_dd;
        std::array<char, 2> mm;
        char seperator_mm;
        std::array<char, 4> yyyy;
    };
    Data data{ { '2', '7' }, '/', { '1', '2' }, '/', { '2', '0', '1', '9' } };
    EXPECT_EQ( 27, ytl::convert_to<uint16_t>( data.dd ) );
    EXPECT_EQ( 12, ytl::convert_to<uint16_t>( data.mm ) );
    EXPECT_EQ( 2019, ytl::convert_to<uint16_t>( data.yyyy ) );
}

```

## Works also for variable-size types

For instance,

```
TEST( TestFixture, CustomEndSplCh_To_Data ) {
    const char * input = "abc,def";

    PtrCsv csv{ input };
    Field< PtrCsv, std::string > data{ csv };
    EXPECT_EQ( "abc", data.yield() );
}
```
