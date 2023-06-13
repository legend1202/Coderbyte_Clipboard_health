# Refactoring(Clipboard He)

## Unit Tests

```
describe("deterministicPartitionKey", () => {
    it("should return a string", () => {
        const result = deterministicPartitionKey({});
        expect(typeof result).toBe("string");
    });

    it("should return the same value for the same input", () => {
        const input = { foo: "bar" };
        const result1 = deterministicPartitionKey(input);
        const result2 = deterministicPartitionKey(input);
        expect(result1).toBe(result2);
    });

    it("should return a different value for different input", () => {
        const input1 = { foo: "bar" };
        const input2 = { bar: "baz" };
        const result1 = deterministicPartitionKey(input1);
        const result2 = deterministicPartitionKey(input2);
        expect(result1).not.toBe(result2);
    });

    it("should return a string with length <= 256", () => {
        const input = { foo: "bar" };
        const result = deterministicPartitionKey(input);
        expect(result.length).toBeLessThanOrEqual(256);
    });
});
```

## Refactored Code

```
const crypto = require("crypto");

exports.deterministicPartitionKey = (event) => {
  const TRIVIAL_PARTITION_KEY = "0";
  const MAX_PARTITION_KEY_LENGTH = 256;

  const getCandidate = () => {
    if (event && event.partitionKey) {
      return event.partitionKey;
    }
    const data = JSON.stringify(event || {});
    return crypto.createHash("sha3-512").update(data).digest("hex");
  };

  let candidate = getCandidate();
  if (typeof candidate !== "string") {
    candidate = JSON.stringify(candidate);
  }
  if (candidate.length > MAX_PARTITION_KEY_LENGTH) {
    candidate = crypto.createHash("sha3-512").update(candidate).digest("hex");
  }
  return candidate || TRIVIAL_PARTITION_KEY;
};
```
