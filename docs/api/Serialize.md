### Serialize

The `Serialize` module provides a number of utility functions for encoding data as strings and decoding data from strings. Two of these functions are pulled up as top-level exports of React URL Query in addition to being available inside `Serialize`: [encode](#encode) and [decode](#decode).

* [`decode(type, encodedValue, [defaultVale])`](#decode)
* [`decodeArray(encodedValue, [entrySeparator])`](#decodeArray)
* [`decodeBoolean(encodedValue)`](#decodeBoolean)
* [`decodeDate(encodedValue)`](#decodeDate)
* [`decodeJson(encodedValue)`](#decodeJson)
* [`decodeNumericArray(encodedValue, [entrySeparator])`](#decodeNumericArray)
* [`decodeNumericObject(encodedValue, [keyValSeparator], [entrySeparator])`](#decodeNumericObject)
* [`decodeObject(encodedValue, [keyValSeparator], [entrySeparator])`](#decodeObject)
* [`encode(type, valueToEncode)`](#encode)
* [`encodeArray(valueToEncode, [entrySeparator])`](#encodeArray)
* [`encodeBoolean(valueToEncode)`](#encodeBoolean)
* [`encodeDate(valueToEncode)`](#encodeDate)
* [`encodeJson(valueToEncode)`](#encodeJson)
* [`encodeNumericArray(valueToEncode, [entrySeparator])`](#encodeNumericArray)
* [`encodeNumericObject(valueToEncode, [keyValSeparator], [entrySeparator])`](#encodeNumericObject)
* [`encodeObject(valueToEncode, [keyValSeparator], [entrySeparator])`](#encodeObject)

### <a id='decode'></a>[`decode(type, encodedValue, [defaultValue])`](#decode)

Decodes a string into a value of the specified type. It does so by delegating to one of the decode*Type* functions based on `type` or by using the custom function passed in through `type`.

#### Arguments

1. `type` (*String|Function|Object*): The type of the data, typically a string from [UrlQueryParamTypes](UrlQueryParamTypes.md). If a function is provided, that function is called with `encodedValue` and `defaultValue` as its arguments. If an object is provided with shape `{ decode }`, `type.decode` is called with `encodedValue` and `defaultValue` as its arguments. Otherwise, if the encoded value is undefined,  defaultValue is used. If no decoder is found to match the string `type`, the encodedValue is returned unmodified.
1. `encodedValue` (*String*): The string representation of the value.
1. `defaultValue` (*Any*): What the value decodes to if `encodedValue` is `undefined`.

#### Returns

(*Any*): The decoded value, type will be based on what was passed into as `type` arg.

#### Examples

```js
decode(UrlQueryParamTypes.number, '94');
// === 94

decode(d => `custom${d}`, '94');
// === 'custom94'

decode({ decode: d => `custom${d}` }, '94');
// === 'custom94'

decode(UrlQueryParamTypes.number, undefined, 137);
// === 137

decode('some-type', '94');
// === '94'
```

### <a id='decodeArray'></a>[`decodeArray(encodedValue, [entrySeparator])`](#decodeArray)

Decodes a string into an array of strings.

#### Arguments

1. `encodedValue` (*String*): The array as a string with entries separated by `entrySeparator`
1. [`entrySeparator`] (*String*): The string used to separate entries in the encoded array. If not provided, defaults to `'_'`.

#### Returns

(*String[]*): An array representation of the encoded value. Each entry is a string.

#### Examples

```js
decodeArray('one_two_three');
// === ['one', 'two', 'three']

decodeArray('one---two---three', '---');
// === ['one', 'two', 'three']
```


### <a id='decodeBoolean'></a>[`decodeBoolean(encodedValue)`](#decodeBoolean)

Decodes a string into a boolean.

#### Arguments

1. `encodedValue` (*String*): The boolean value as a string, where `'1'` is `true`, `'0'` is false, and everything else is `undefined`.

#### Returns

(*Boolean*): The boolean representation of the encoded value.

#### Examples

```js
decodeBoolean('1');
// === true

decodeBoolean('0');
// === false

decodeBoolean('true');
// === undefined
```

### <a id='decodeDate'></a>[`decodeDate(encodedValue)`](#decodeDate)

Decodes a string into a Date. The string can be of form `YYYY`, `YYYY-MM`, or `YYYY-MM-DD`.

#### Arguments

1. `encodedValue` (*String*): The Date value as a string, can be of form `YYYY`, `YYYY-MM`, or `YYYY-MM-DD`.

#### Returns

(*Date*): The Date representation of the encoded value. If the input string is invalid, it returns `undefined`.

#### Examples

```js
decodeDate('2014-04-21');
// === new Date(2014, 3, 21)

decodeDate('2015');
// === new Date(2015 0, 1);
```


### <a id='decodeJson'></a>[`decodeJson(encodedValue)`](#decodeJson)

Decodes a string into javascript based on `JSON.parse`.


#### Arguments

1. `encodedValue` (*String*): The javascript data to parse.


#### Returns

(*Any*): The javascript representation of the encoded value. If the input string is invalid, it returns `undefined`.

#### Examples

```js
decodeJson('{"foo": "bar", "jim": ["grill"]}');
// === {'foo': 'bar', 'jim': ['grill']}

decodeJson('["one", "two", "three"]');
// === ['one', 'two', 'three']
```

### <a id='decodeNumericArray'></a>[`decodeNumericArray(encodedValue, [entrySeparator])`](#decodeNumericArray)

Decodes a string into an array of numbers.

#### Arguments

1. `encodedValue` (*String*): The array as a string with entries separated by `entrySeparator`
1. [`entrySeparator`] (*String*): The string used to separate entries in the encoded array. If not provided, defaults to `'_'`.

#### Returns

(*String[]*): An array representation of the encoded value. Each entry is a number.

#### Examples

```js
decodeNumericArray('1_2_3');
// === [1, 2, 3]

decodeNumericArray('1---2---3', '---');
// === [1, 2, 3]
```


### <a id='decodeNumericObject'></a>[`decodeNumericObject(encodedValue, [keyValSeparator], [entrySeparator])`](#decodeNumericObject)

Decodes a string into an object. Supports simple, flat objects where the values are numbers.

#### Arguments

1. `encodedValue` (*String*): The object value as a string where keys and values are separated by `keyValSeparator` and entries are separated by `entrySeparator`.
1. [`keyValSeparator`] (*String*): The string used to separate keys from values in the encoded object. If not provided, defaults to `'-'`.
1. [`entrySeparator`] (*String*): The string used to separate entries in the encoded object. If not provided, defaults to `'_'`.

#### Returns

(*Object*): The Object representation of the encoded value.

#### Examples

```js
decodeNumericObject('foo-44_boo-51');
// === { foo: 44, boo: 51 }

decodeNumericObject('foo---44___boo---51', '---', '___');
// === { foo: 44, boo: 51 }
```


### <a id='decodeObject'></a>[`decodeObject(encodedValue, [keyValSeparator], [entrySeparator])`](#decodeObject)

Decodes a string into an object. Only supports simple, flat objects where the values are strings.

#### Arguments

1. `encodedValue` (*String*): The object value as a string where keys and values are separated by `keyValSeparator` and entries are separated by `entrySeparator`.
1. [`keyValSeparator`] (*String*): The string used to separate keys from values in the encoded object. If not provided, defaults to `'-'`.
1. [`entrySeparator`] (*String*): The string used to separate entries in the encoded object. If not provided, defaults to `'_'`.

#### Returns

(*Object*): The Object representation of the encoded value.

#### Examples

```js
decodeObject('foo-bar_boo-baz');
// === { foo: 'bar', boo: 'baz' }

decodeObject('foo---bar___boo---baz', '---', '___');
// === { foo: 'bar', boo: 'baz' }
```

### <a id='encode'></a>[`encode(type, valueToEncode)`](#encode)

Encodes a value of the specified type as a string. It does so by delegating to one of the encode*Type* functions based on `type` or by using the custom function passed in through `type`.

#### Arguments

1. `type` (*String|Function|Object*): The type of the data, typically a string from [UrlQueryParamTypes](UrlQueryParamTypes.md). If a function is provided, that function is called with `valueToEncode` as its argument. If an object is provided with shape `{ encode }`, `type.encode` is called with `valueToEncode` as its argument. If no encoder is found to match the string `type`, the `valueToEncode` is returned unmodified.
1. `valueToEncode` (*Any*): The value to encode as a string. Should match the specified type.

#### Returns

(*String*): The value encoded as a string based on the type.

#### Examples

```js
encode(UrlQueryParamTypes.number, 94);
// === '94'

encode(UrlQueryParamTypes.object, { test: 'ing', foo: 'bar' });
// === 'test-ing_foo-bar'

encode(d => d.substring(6), 'custom94');
// === '94'

encode({ encode: d => d.substring(6) }, 'custom94');
// === '94'

encode('some-type', '94');
// === '94'
```


### <a id='encodeArray'></a>[`encodeArray(valueToEncode, [entrySeparator])`](#encodeArray)

Encodes an array as a string.

#### Arguments

1. `valueToEncode` (*String[]|Number[]|Boolean[]*): The array to be encoded as a string.
1. [`entrySeparator`] (*String*): The string used to separate entries in the encoded array. If not provided, defaults to `'_'`.

#### Returns

(*String*): The array represented as a string where entries in the array are separated by `entrySeparator`.

#### Examples

```js
encodeArray(['one', 'two', 'three']);
// === 'one_two_three'

encodeArray(['one', 'two', 'three'], '---');
// === 'one---two---three'
```

### <a id='encodeBoolean'></a>[`encodeBoolean(valueToEncode)`](#encodeBoolean)

Encodes a boolean as a string, where undefined values get `undefined`, truthy values get `'1'`, everything else gets `'0'`.

#### Arguments

1. `valueToEncode` (*Boolean*): The boolean value to be encoded as a string.

#### Returns

(*String*): The encoded string where undefined values get `undefined`, truthy values get `'1'`, everything else gets `'0'`.

#### Examples

```js
encodeBoolean(true);
// === '1'

encodeBoolean(0);
// === '0'

encodeBoolean(false);
// === '0'

encodeBoolean('something');
// === '1'

encodeBoolean();
// === undefined
```


### <a id='encodeDate'></a>[`encodeDate(valueToEncode)`](#encodeDate)

Encodes a Date as a string in `YYYY-MM-DD` format.

#### Arguments

1. `valueToEncode` (*Date*): The Date value to be encoded as a string.

#### Returns

(*String*): The encoded string in `YYYY-MM-DD` format.

#### Examples

```js
encodeDate(new Date(2014, 1, 15));
// === '2014-02-15'

encodeDate(new Date('Sun Sep 18 2016 20:00:00'));
// === '2016-09-18'
```


### <a id='encodeJson'></a>[`encodeJson(valueToEncode)`](#encodeJson)

Encodes javascript data into a string through `JSON.stringify`.

#### Arguments

1. `valueToEncode` (*Any*):

#### Returns

(*String*): The javascript data encoded as a string. If the input was nully, returns `undefined`.

#### Examples

```js
encodeJson({'foo': 'bar', 'jim': ['grill']});
// === '{"foo": "bar", "jim": ["grill"]}'

encodeJson(['one', 'two', 'three']);
// === '["one", "two", "three"]'

encodeJson(null);
// === undefined
```


### <a id='encodeNumericArray'></a>[`encodeNumericArray(valueToEncode, [entrySeparator])`](#encodeNumericArray)

Encodes an array as a string.

#### Arguments

1. `valueToEncode` (*String[]|Number[]|Boolean[]*): The array to be encoded as a string.
1. [`entrySeparator`] (*String*): The string used to separate entries in the encoded array. If not provided, defaults to `'_'`.

#### Returns

(*String*): The array represented as a string where entries in the array are separated by `entrySeparator`.

#### Examples

```js
encodeNumericArray([1, 2, 3]);
// === '1_2_3'

encodeNumericArray([1, 2, 3], '---');
// === '1---2---3'
```


### <a id='encodeNumericObject'></a>[`encodeNumericObject(valueToEncode, [keyValSeparator], [entrySeparator])`](#encodeNumericObject)

Encodes a flat javascript object as a string. Supports flat objects where the values are numbers.

#### Arguments

1. `valueToEncode` (*Object*): The javascript object to encode.
1. [`keyValSeparator`] (*String*): The string used to separate keys from values in the encoded object. If not provided, defaults to `'-'`.
1. [`entrySeparator`] (*String*): The string used to separate entries in the encoded object. If not provided, defaults to `'_'`.

#### Returns

(*String*): The object encoded as a string.

#### Examples

```js
encodeNumericObject({ foo: 94, boo: 137 });
// === 'foo-94_boo-137'

encodeNumericObject({ foo: 94, boo: 137 }, '---', '___');
// === 'foo---94___boo---137'
```


### <a id='encodeObject'></a>[`encodeObject(valueToEncode, [keyValSeparator], [entrySeparator])`](#encodeObject)

Encodes a flat javascript object as a string. Only supports flat objects where the values are strings.

#### Arguments

1. `valueToEncode` (*Object*): The javascript object to encode.
1. [`keyValSeparator`] (*String*): The string used to separate keys from values in the encoded object. If not provided, defaults to `'-'`.
1. [`entrySeparator`] (*String*): The string used to separate entries in the encoded object. If not provided, defaults to `'_'`.

#### Returns

(*String*): The object encoded as a string.

#### Examples

```js
encodeObject({ foo: 'bar', boo: 'baz' });
// === 'foo-bar_boo-baz'

encodeObject({ foo: 'bar', boo: 'baz' }, '---', '___');
// === 'foo---bar___boo---baz'
```
