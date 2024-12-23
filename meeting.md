# ICRCX recently update

- Add new error code for failed by validation canister

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

- We keep validation per request
  - its simpler to keep this separate from sequence which stops if ANY (validation or response) fails
  - we get to know if request fails because of valdiation: this will be useful if the relying party wants to know that all the requests were successful before doing other things
- Error code
  - new code when request fails because of validation failure
- Sea snake’s response (and similar options)
  - option1: make call to relying party canister
  - option2: make call to target canister
  - option3: target canisters add more error codes to the response, showing that request was not processed successfully
  - thoughts:
    - between option 2 and option3, we’d prefer option 3 because there is no real need to make another call if target canisters are going to make changes anyways
    - between option 1, 2, and 3, we think option 1 is more realistic. option 2 and 3 may take years to get sufficient adoption. we could pursue them. But we shouldn’t expect it to materialize in short time.
