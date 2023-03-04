# Prevent Unexpected Results
No code is perfect! Think of possible situations that can happen:
* What will make this code crash?
* What data must be correct to safely run the following code?
---
# While loop
Must have limitations! So it will never hang the system.
```C#
int limit = 20;
while (condition == true && limit > 0)
{
  // your code
  limit--;
}
```
---
