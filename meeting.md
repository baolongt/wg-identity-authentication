# ICRCX recently update

- Add new error code for failed by validation canister

- We're thinking do another ICRC to return the validate response in a standard way

# Open topic

There is a feedback about changing the JSON RPC schema look like

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "icrcX_batch_call_canisters",
  "params": {
    "sender": "b7gqo-ulk5n-2kpo7-oalt7-p2kyl-o4j5l-kiuwo-eeybr-dab4l-ur6up-pqe",
    "validate": {
      canisterId: "xyzzz-fqaaa-aaaao-a2hlq-cai"
      method: "validate_batch_approve_swap_bridge"
    },
    "requests": [
    {
        "id": "1",
        "canisterId": "eeddf-fqaaa-aaaao-a2hlq-ca",
        "method": "icrc2_approve",
        "arg": "RElETARte24AbAKzsNrDA2ithsqDBQFsA/vKAQKi3pTrBgHYo4yoDX0BAwEdV+ztKgq7E4l1ffuTuwEmw8AtYSjlrJ+WLO5ofQIAAMgB"
      },
      {
        "id": "2",
        "canisterId": "aaabb-fqaaa-aaaao-a2hlq-ca",
        "method": "icrc2_approve",
        "arg": "RElETARte24AbAKzsNrDA2ithsqDBQFsA/vKAQKi3pTrBgHYo4yoDX0BAwEdV+ztKgq7E4l1ffuTuwEmw8AtYSjlrJ+WLO5ofQIAAMgB"
      },
      {
        "id": "3",
        "canisterId": "xyzzz-fqaaa-aaaao-a2hlq-ca",
        "method": "swap",
        "arg": "RElETARte24AbAKzsNrDA2ithsqDBQFsA/vKAQKi3pTrBgHYo4yoDX0BAwEdV+ztKgq7E4l1ffuTuwEmw8AtYSjlrJ+WLO5ofQIAAMgB",
        "waitFor": ["1", "2"]
      },
      ,
      {
        "id": "4",
        "canisterId": "bbbbb-fqaaa-aaaao-a2hlq-ca",
        "method": "bridge_to_eth",
        "arg": "RElETARte24AbAKzsNrDA2ithsqDBQFsA/vKAQKi3pTrBgHYo4yoDX0BAwEdV+ztKgq7E4l1ffuTuwEmw8AtYSjlrJ+WLO5ofQIAAMgB",
        "waitFor": ["3"]
      }
    ]
  }
}
```

Current schema

- Current there is no way to know the method to call the canister but we can easily fix by adding `method` property for the request. Or this can be an standard so we have a fixed method name like ICRC21

```json
{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "icrcX_batch_call_canisters",
  "params": {
    "sender": "b7gqo-ulk5n-2kpo7-oalt7-p2kyl-o4j5l-kiuwo-eeybr-dab4l-ur6up-pqe",
    "requests": [
      {
        "id": "1",
        "canisterId": "eeddf-fqaaa-aaaao-a2hlq-ca",
        "method": "icrc2_approve",
        "arg": "RElETARte24AbAKzsNrDA2ithsqDBQFsA/vKAQKi3pTrBgHYo4yoDX0BAwEdV+ztKgq7E4l1ffuTuwEmw8AtYSjlrJ+WLO5ofQIAAMgB"
      },
      {
        "id": "2",
        "canisterId": "aaabb-fqaaa-aaaao-a2hlq-ca",
        "method": "icrc2_approve",
        "arg": "RElETARte24AbAKzsNrDA2ithsqDBQFsA/vKAQKi3pTrBgHYo4yoDX0BAwEdV+ztKgq7E4l1ffuTuwEmw8AtYSjlrJ+WLO5ofQIAAMgB"
      },
      {
        "id": "3",
        "canisterId": "xyzzz-fqaaa-aaaao-a2hlq-ca",
        "method": "swap",
        "arg": "RElETARte24AbAKzsNrDA2ithsqDBQFsA/vKAQKi3pTrBgHYo4yoDX0BAwEdV+ztKgq7E4l1ffuTuwEmw8AtYSjlrJ+WLO5ofQIAAMgB",
        "waitFor": ["1", "2"],
        "validateCanister": "xyzzz-fqaaa-aaaao-a2hlq-ca"
      },
      {
        "id": "4",
        "canisterId": "bbbbb-fqaaa-aaaao-a2hlq-ca",
        "method": "bridge_to_eth",
        "arg": "RElETARte24AbAKzsNrDA2ithsqDBQFsA/vKAQKi3pTrBgHYo4yoDX0BAwEdV+ztKgq7E4l1ffuTuwEmw8AtYSjlrJ+WLO5ofQIAAMgB",
        "waitFor": ["3"]
      }
    ]
  }
}
```
