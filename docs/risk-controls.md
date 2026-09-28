# Risk controls

Every check that stands between a signal and real money.

The principle behind all of them: **the engine should fail towards not trading.**
When something is missing, unclear or over a limit, the safe outcome is a
simulated order or no order at all, never a real one.

---

## Paper trading is the default

Every time the engine starts, every index is in paper trading. The whole engine
runs against the live market exactly as it would for real, except the orders are
simulated instead of sent to the broker.

Real trading has to be switched on deliberately, one index at a time, from the
operator console.

There is a second guard behind this. If the settings for an index come through
blank, meaning paper trading off, the loss check off and a loss limit of zero,
the engine treats it as a mistake and forces that index back to paper. A blank
form can never turn into real trading.

---

## The daily loss limit

Each index has a loss limit for the day, in rupees. Once the running profit and
loss for the day falls to that limit, the index is switched to paper trading
for the rest of the day.

It still keeps running and keeps recording what it would have done. That shows
whether stopping was the right call. It just can't lose any more real money.

---

## The profit lock

On a good day, the loss limit moves up to protect the gains.

Once the day's profit passes ₹4,000, the loss limit rises in steps of ₹1,000 as
profit grows. It only ever moves up. On a day that reaches ₹8,000 in profit,
the limit is ₹4,000. If profit falls back to ₹4,000, the engine switches to
paper and keeps the rest.

---

## One position per index

Only one position can be open on an index at a time. When a trade is entered,
the index is locked. It is released only when that position is fully closed.

This stops the engine piling into the same move several times over when signals
arrive close together.

---

## Early-failure exit

If a trade drops to 80% of its entry price within the first three minutes, the
engine exits it straight away without waiting for the stop loss.

A scalp that goes wrong that fast rarely recovers. Waiting only makes the loss
bigger.

---

## Stale and cancelled signals

**Stale setups are ignored.** The engine records when it started watching each
contract. A setup that formed before that time is ignored, because it was based
on prices the engine never saw.

**Cancelled setups are dropped.** If the chart sends a cancel before the price
confirms the setup, the waiting signal is cleared.

**Dropped contracts are marked.** When the engine stops watching a contract
because the index has moved on, any signal on it is marked as out of scope and
can't trigger.

---

## Side blocking

Each index can be set to allow calls only, puts only, both blocked, or
everything allowed. This is the quickest way to stop trading one direction
without stopping the engine. For example, on a day with a strong trend, or
around a major announcement.

---

## Order safety

Every real order is confirmed with the broker after it is placed. The engine
does not assume an order went through.

If the broker says there have been too many requests, the engine waits longer
before each retry. Any other failure is retried a fixed number of times, then
reported.

---

## A human can always step in

From the operator console, during market hours, the operator can:

- move the stop loss on any open position by dragging it
- exit part of a position, or all of it
- switch any index between real and paper trading
- change any setting for any index

Every change takes effect on the next price update.

---

## Alerts

A Telegram message goes out when the engine starts or stops, on every entry and
exit, and when a loss limit is hit. The operator doesn't have to be watching the
screen to know what the engine did.
