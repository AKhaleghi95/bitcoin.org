{% comment %}
This file is licensed under the MIT License (MIT) available on
http://opensource.org/licenses/MIT.
{% endcomment %}
{% assign filename="_includes/ref/bitcoin-core/rest/requests/get_block-notxdetails.md" %}

##### GET Block/NoTxDetails
{% include helpers/subhead-links.md %}

{% assign summary_restGetBlock-noTxDetails="gets a block with a particular header hash from the local block database either as a JSON object or as a serialized block.  The JSON object includes TXIDs for transactions within the block rather than the complete transactions [GET block][rest get block] returns." %}

{% autocrossref %}

The `GET block<!--noref-->/notxdetails` operation {{summary_restGetBlock-noTxDetails}}

*Request*

{% highlight text %}
GET /block/notxdetails/<hash>.<format>
{% endhighlight %}

*Parameter #1---the header hash of the block to retrieve*

| Name             | Type         | Presence                    | Description
|------------------|--------------|-----------------------------|----------------
| Header Hash      | path (hex)   | Required<br>(exactly 1)     | The hash of the header of the block to get, encoded as hex in RPC byte order
{:.ntpd}

*Parameter #2---the output format*

| Name             | Type         | Presence                    | Description
|------------------|--------------|-----------------------------|----------------
| Format           | suffix       | Required<br>(exactly 1)     | Set to `.json` for decoded block contents in JSON, or `.bin` or `hex` for a serialized block in binary or hex
{:.ntpd}

*Response as JSON*

| Name                     | Type              | Presence                    | Description
|--------------------------|-------------------|-----------------------------|----------------
| Result                   | object            | Required<br>(exactly 1)     | An object containing the requested block
| →<br>`hash`              | string (hex)      | Required<br>(exactly 1)     | The hash of this block's block header encoded as hex in RPC byte order.  This is the same as the hash provided in parameter #1
| →<br>`confirmations`     | number (int)      | Required<br>(exactly 1)     | The number of confirmations the transactions in this block have, starting at 1 when this block is at the tip of the best block chain.  This score will be -1 if the the block is not part of the best block chain
| →<br>`size`              | number (int)      | Required<br>(exactly 1)     | The size of this block in serialized block format, counted in bytes
| →<br>`height`            | number (int)      | Required<br>(exactly 1)     | The height of this block on its block chain
| →<br>`version`           | number (int)      | Required<br>(exactly 1)     | This block's version number.  See [block version numbers][section block versions]
| →<br>`merkleroot`        | string (hex)      | Required<br>(exactly 1)     | The merkle root for this block, encoded as hex in RPC byte order
| →<br>`tx`                | array             | Required<br>(exactly 1)     | An array containing all transactions in this block.  The transactions appear in the array in the same order they appear in the serialized block
| → →<br>TXID              | string (hex)      | Required<br>(1 or more)     | The TXID of a transaction in this block, encoded as hex in RPC byte order
| →<br>`time`              | number (int)      | Required<br>(exactly 1)     | The value of the *time* field in the block header, indicating approximately when the block was created
| →<br>`nonce`             | number (int)      | Required<br>(exactly 1)     | The nonce which was successful at turning this particular block into one that could be added to the best block chain
| →<br>`bits`              | string (hex)      | Required<br>(exactly 1)     | The value of the *nBits* field in the block header, indicating the target threshold this block's header had to pass
| →<br>`difficulty`        | number (real)     | Required<br>(exactly 1)     | The estimated amount of work done to find this block relative to the estimated amount of work done to find block 0
| →<br>`chainwork`         | string (hex)      | Required<br>(exactly 1)     | The estimated number of block header hashes miners had to check from the genesis block to this block, encoded as big-endian hex
| →<br>`previousblockhash` | string (hex)      | Required<br>(exactly 1)     | The hash of the header of the previous block, encoded as hex in RPC byte order
| →<br>`nextblockhash`     | string (hex)      | Optional<br>(0 or 1)        | The hash of the next block on the best block chain, if known, encoded as hex in RPC byte order
{:.ntpd}

*Examples from Bitcoin Core 0.10.0*

Request a block in hex-encoded serialized block format:

{% highlight bash %}
curl http://localhost:18332/rest/block/notxdetails/000000000fe549a89848c76070d4132872cfb6efe5315d01d7ef77e4900f2d39.hex
{% endhighlight %}

Result (wrapped):

{% highlight text %}
02000000df11c014a8d798395b5059c722ebdf3171a4217ead71bf6e0e99f4c7\
000000004a6f6a2db225c81e77773f6f0457bcb05865a94900ed11356d0b7522\
8efb38c7785d6053ffff001d005d437001010000000100000000000000000000\
00000000000000000000000000000000000000000000ffffffff0d03b4770301\
64062f503253482fffffffff0100f9029500000000232103adb7d8ef6b63de74\
313e0cd4e07670d09a169b13e4eda2d650f529332c47646dac00000000
{% endhighlight %}

Get the same block in JSON:

{% highlight bash %}
curl http://localhost:18332/rest/block/notxdetails/000000000fe549a89848c76070d4132872cfb6efe5315d01d7ef77e4900f2d39.json
{% endhighlight %}

Result (whitespaced added):

{% highlight json %}
{
    "hash": "000000000fe549a89848c76070d4132872cfb6efe5315d01d7ef77e4900f2d39",
    "confirmations": 91807,
    "size": 189,
    "height": 227252,
    "version": 2,
    "merkleroot": "c738fb8e22750b6d3511ed0049a96558b0bc57046f3f77771ec825b22d6a6f4a",
    "tx": [
        "c738fb8e22750b6d3511ed0049a96558b0bc57046f3f77771ec825b22d6a6f4a"
    ],
    "time": 1398824312,
    "nonce": 1883462912,
    "bits": "1d00ffff",
    "difficulty": 1.0,
    "chainwork": "000000000000000000000000000000000000000000000000083ada4a4009841a",
    "previousblockhash": "00000000c7f4990e6ebf71ad7e21a47131dfeb22c759505b3998d7a814c011df",
    "nextblockhash": "00000000afe1928529ac766f1237657819a11cfcc8ca6d67f119e868ed5b6188"
}
{% endhighlight %}

*See also*

* [GET Block][rest get block]: {{summary_restGetBlock}}
* [GetBlock][rpc getblock] RPC: {{summary_getBlock}}
* [GetBlockHash][rpc getblockhash] RPC: {{summary_getBlockHash}}
* [GetBestBlockHash][rpc getbestblockhash] RPC: {{summary_getBestBlockHash}}

{% endautocrossref %}
