## SP XML
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

Rust community library for serializaing/deserializing IBM Safer Payments® XML messages.

### Usage
```toml
[dependencies]
sp-xml = "0.1"
```

```rust
use sp_xml::{SPRequest, SPResponse};

let s = r#"
    <IRIS Version="1" Message="ModelRequest" MessageTypeId="60" MessageId="0af87c75503b4401">
        <msgSubType>iddqd</msgSubType>
        <msgType>aaaa</msgType>
        <msisdnA>231231</msisdnA>
        <msisdnB>54656456</msisdnB>
        <partNumber>127</partNumber>
        <sessionId>bbbbb</sessionId>
        <siebelId>ccccc</siebelId>
        <smsBody>ddddd</smsBody>
        <smsId>eee</smsId>
        <timestamp>2020-04-27 12:00:00</timestamp>
        <vlr>36028797018963968</vlr>
    </IRIS>"#;

// Deserializing request
let req = SPRequest::new(s.as_bytes());
println!("{:?}", req);

// Applying logic on deserialized request, e.g. generating and assinging Message ID:
req.gen_message_id();

// Serializing request
let msg : String = req.serialize().unwrap();

// Sending the data over TCP stream:
s.write_all(&msg.as_bytes()).await?;
```

```rust
let s = r##"
    <IRIS Version="1" Message="ModelResponse" IrisInstance="INSTANCE_1_(DS-PR-" MessageTypeId="60" SystemTime="2020-05-18 23:39:19" UniqueRecordId="1882261" MessageId="0af87c75503b4401" Merging="0" InstanceStatus="Ok" Latency="1.15" ErrorCode="0"></IRIS>
    "##;

// Deserializing response
let resp = SPResponse::new(s.as_bytes());
println!("{:?}", resp);

// Applying logic on deserialized response, e.g. checking message ID:
println!("{:?}", resp.message_id);

// Serializing Response
let serialized = res.serialize().unwrap();
println!("{:?}", serialized);

// Sending serialized response in HTTP payload
Ok(HttpResponse::Ok()
    .content_type("text/xml")
    .body(serialized))
```

Check [lakgves](https://github.com/timgabets/lakgves) for more examples.
