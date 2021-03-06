##### Table of Contents
[Functor](#functor)
[Applicative Functor](#applicative-functor)
[Monoid](#monoid)

# Functor
> Modifications on boxed values
> Functor - box with value, that used in fmap function

## Interface

```haskell
class Functor f where
    fmap :: (a -> b) -> f a -> f b
```


## Example implementation

```haskell
instance Functor Maybe where
    fmap f (Just x) = Just (f x)
    fmap f Nothing = Nothing
```

## Laws

### [1] fmap id = id

```haskell
fmap id (Just 3) = Just 3
```

### [2] fmap (f . g) = fmap f . fmap g

```haskell
fmap (f . g) (Just x) = Just (f (g x))
```

# Applicative Functor
> Same function, but with function[s] as value

## Interface

```haskell
class (Functor f) => Applicative f where
    pure :: a -> f a
    (<*>) :: f (a -> b) -> f a -> f b
```

## Example implementation

```haskell
instance Applicative Maybe where
    pure = Just
    Nothing <*> _ = Nothing
    (Just f) <*> something = fmap f something
```

## Example usage

```haskell
pure (+) <*> Just 3 <*> Just 5
-- fmap = <$>
(+) <$> Just 3 <*> Just 5

[(*0),(+100),(^2)] <*> [1,2,3]

[(*),(+)] <*> [1,2], [3,4]

(*) <$> [1,2], [3,4]
```

# Monoid
> Concatenable box

## Interface
```haskell
class Monoid m where  
    mempty :: m  
    mappend :: m -> m -> m  
    mconcat :: [m] -> m  
    mconcat = foldr mappend mempty  
```

## Laws
### [1] mempty `mappend` x = x
### [2] x `mappend` mempty = x
### [3] (x `mappend` y) `mappend` z = x `mappend` (y `mappend` z)


## Example implementation
```haskell
instance Monoid [a] where  
    mempty = []  
    mappend = (++)  
```

## Example usage
```haskell
[1,2,3] `mappend` [4,5,6]
-- [1,2,3,4,5,6]

("one" `mappend` "two") `mappend` "tree"  
-- "onetwotree"

mconcat [[1,2],[3,6],[9]]
-- [1,2,3,6,9]
```
