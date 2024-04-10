# Get Min &amp; Max Values

### C# Solution

```C#
Tuple<int, int> GetMinAndMax(IEnumerable<int> input)
{
    if (!input.Any())
    {
        throw new InvalidOperationException(
            $"The input collection cannot be empty.");
    }
    int min = input.First();
    int max = input.First();

    foreach (var number in input)
    {
        if (number > max)
        {
            max = number;
        }
        if (number < min)
        {
            min = number;
        }
    }

    return new Tuple<int, int>(min, max);
}
```