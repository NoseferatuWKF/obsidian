# API

## [memo](https://react.dev/reference/react/memo)
Cache components to skip re-rendering
```tsx
const Spec = memo((props: SpecProps) => {
// ...
}
```

## [createContext](https://react.dev/reference/react/createContext)
provides context for components to read from
```tsx
// contexts.ts
export const ManufacturerContext = createContext('Toyota');

// App.ts
<ManufacturerContext.Provider value={manufacturer}>
	<Engine/>
</ManufacturerContext.Provider>

```

# Hooks

## [useState](https://react.dev/reference/react/useState)
Add state to a component
```tsx
const [spec, setSpecs] = useState<Specs>(null)
```

## [useContext](https://react.dev/reference/react/useContext)
Use to read from createContext
```tsx
const Engine = () => {
	const manufacturer = useContext(ManufacturerContext)
	const engines = useMemo(() => getEngines(manufacturer))

	return (
		<>
			{engines.map(x => <Spec key={x} name={x}/>)}
		</>
	)
}
```

## [useEffect](https://react.dev/reference/react/useEffect)
Handle side effects when connecting components with external systems (i.e: backend APIs)
```tsx
useEffect(() => {
	setSpec(engine(name))
}, [engine])
```

## [useLayoutEffect](https://react.dev/reference/react/useLayoutEffect)
Handle side effect before browser starts painting
```tsx
useLayoutEffect(() => {
	setSpec(engine(name))
}, [engine])
```

## [useMemo](https://react.dev/reference/react/useMemo)
Call function and cache the results
```tsx
const App = () => {
	const engines = useMemo(getEngines)
	return (
		<div>
			{engines.map(x => <Card key={x} name={x}></Card>)}
		</div>
	)
}
```

## [useCallback](https://react.dev/reference/react/useCallback)
Cache function definitions
```tsx
const engine = useCallback((name) => getEngineSpec(name), [name])
```

# Escape Hatches

## [ref](https://react.dev/learn/manipulating-the-dom-with-refs)
```tsx
// useRef hook in component
const myRef = useRef(null);

// pass hook to ref attribute
<div ref={myRef}></div>

// now we can use the browser API to do stuff on the ref
myRef.current.scrollIntoView()
```
# Packages

## React Query