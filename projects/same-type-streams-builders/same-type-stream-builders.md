This widget produces nested `StreamBuilder` widgets for your streams.

## Usage

```dart
final stream1 = Stream<int>.periodic(Duration(seconds: 1), (x) => x);
final stream2 = Stream<int>.periodic(Duration(seconds: 2), (x) => x);
final stream3 = Stream<int>.periodic(Duration(seconds: 3), (x) => x);


Widget build(BuildContext context) {
  return SameTypeStreamsBuilder<int>(
    streams: [stream1, stream2, stream3],
    builder: (context, snapshots) {
      return Text(
        'stream1: ${snapshots[0].data} - stream2: ${snapshots[1].data} - stream3: ${snapshots[2].data}',
      );
    },
  );
}
```

## Additional information

This widget is best if your streams are of the same type and their count is not known
at compile time.

If your streams are of different types,
you technically may use `dynamic` as their common type and then cast the snapshot data to
your types. However, in this case it is best to use
[multiple_stream_builder package](https://pub.dev/packages/multiple_stream_builder)
which has separate type for each stream (unrelated to me).
