# Verify Randomness

The hashed server seed for a bet is revealed on the previous bet, and the server seed is revealed when the bet is completed.

You can view bet data at `/api/bet/{bet number}`

## Randomness Calculation

The roll number is calculated like the following psudo-code:

```javascript
    const hashedValue = sha512(`${serverSeed}+${clientSeed}+${nonce}+${lootboxId}`)
    let index = 0;
    let result;
    do {
        result = parseInt(hashedValue.substring(index * 5, index * 5 + 5), 16);
        index += 1;
        if (index * 5 + 5 > 129) {
            result = 9999;
            break;
        }
    } while (result >= 1e6);
    return Math.floor([result % 1e4] * 1e-2);
```

The roll corresponds with their Tier in its range and the reward (if one) is the roll as an integer modular to the rewards in the tier.
