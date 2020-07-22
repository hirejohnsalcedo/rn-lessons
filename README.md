# README

## Guard against bad svg
```
<View pointerEvents="none">
  <SvgXml xml={xml} />
</View>
```

## Nullish Coalescing Operator usage
Do not use the nullish coalescing operator `??`, when the falsy left-side expression should not be evaluated to `0` or `""`. (tags: avoiding unintended outcomes )

## Separation of concerns
Do not pass down implementation details from containers to screens or subcomponents (e.g. `componentId` ) if it can be defined at the container level. This improves readability and exposes intentionality when passing down props from the container. (tags: readability, segregation of responsibility)

For example, compare the two styles:

### Passing named function:
```
const MyContainer = (props) => {
  const handleNavigate = () => {
    Navigation.push(props.componentId, {...} )
  }
  return <MyScreen onNavigate={handleNavigate} />
}
const MyScreen = (props) => {
  return <Button onPress={props.onNavigate} />
}
```

### Passing implementation detail:
```
const MyContainer = (props) => {
  return <MyScreen componentId={props.componentId} />
}
const MyScreen = (props) => {
  const handleNavigate = () => {
    Navigation.push(props.componentId, {...} )
  }
  return <Button onPress={handleNavigate} />
}
```

In the second example, it takes a reviewer to go up to the `MyScreen` component before understanding why the `componentId` prop is being passed. Whereas in the first example, it's made more clear that the container just wants to give the subcomponents a way to navigate. Presentational components don't need to know about other components, states, or navigation. They're only concerned with receiving some data, and being able to display that data in a nice way.
