# Component Templates Reference

Load this file when you need to generate a component and want a concrete starting point. Adapt the template to the project's existing patterns (e.g., if the project uses arrow functions everywhere, match that).

---

## Shared Component Template (React Native)

```tsx
import React from 'react';
import { View, Text, StyleSheet } from 'react-native';

export interface ComponentNameProps {
  // Define props here
}

function ComponentName({ ...props }: ComponentNameProps) {
  return (
    <View style={styles.container}>
      {/* Component content */}
    </View>
  );
}

const styles = StyleSheet.create({
  container: {},
});

export default ComponentName;
```

## Shared Component Template (React Web)

```tsx
import React from 'react';

export interface ComponentNameProps {
  // Define props here
}

function ComponentName({ ...props }: ComponentNameProps) {
  return (
    <div>
      {/* Component content */}
    </div>
  );
}

export default ComponentName;
```

---

## Container Component Template (React Native)

```tsx
import React from 'react';
import { View, ActivityIndicator, StyleSheet } from 'react-native';

export interface ComponentNameContainerProps {
  // Props that the container receives from its parent
}

function ComponentNameContainer({ ...props }: ComponentNameContainerProps) {
  // API calls, data fetching, state management
  // const { data, isLoading, error } = useQuery(...)

  // if (isLoading) return <ActivityIndicator />;
  // if (error) return <ErrorDisplay error={error} />;

  return (
    <View style={styles.container}>
      {/* Render shared/presentational children, passing data as props */}
    </View>
  );
}

const styles = StyleSheet.create({
  container: {},
});

export default ComponentNameContainer;
```

## Container Component Template (React Web)

```tsx
import React from 'react';

export interface ComponentNameContainerProps {
  // Props that the container receives from its parent
}

function ComponentNameContainer({ ...props }: ComponentNameContainerProps) {
  // API calls, data fetching, state management
  // const { data, isLoading, error } = useQuery(...)

  // if (isLoading) return <LoadingSpinner />;
  // if (error) return <ErrorDisplay error={error} />;

  return (
    <div>
      {/* Render shared/presentational children, passing data as props */}
    </div>
  );
}

export default ComponentNameContainer;
```

---

## Form Component Template (React Native)

```tsx
import React from 'react';
import { View, TextInput, Pressable, Text, StyleSheet } from 'react-native';

export interface ComponentNameFormProps {
  onSubmit: (data: FormData) => void;
  // initialValues, loading state, etc.
}

interface FormData {
  // Define form field types
}

function ComponentNameForm({ onSubmit, ...props }: ComponentNameFormProps) {
  // Form state management
  // const { control, handleSubmit, formState } = useForm<FormData>();

  return (
    <View style={styles.container}>
      {/* Form fields */}
      <Pressable onPress={() => {/* handleSubmit(onSubmit) */}}>
        <Text>Submit</Text>
      </Pressable>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {},
});

export default ComponentNameForm;
```

## Form Component Template (React Web)

```tsx
import React from 'react';

export interface ComponentNameFormProps {
  onSubmit: (data: FormData) => void;
  // initialValues, loading state, etc.
}

interface FormData {
  // Define form field types
}

function ComponentNameForm({ onSubmit, ...props }: ComponentNameFormProps) {
  // Form state management
  // const { register, handleSubmit, formState } = useForm<FormData>();

  return (
    <div>
      {/* Form fields */}
      <button type="button" onClick={() => {/* handleSubmit(onSubmit) */}}>
        Submit
      </button>
    </div>
  );
}

export default ComponentNameForm;
```

---

## Barrel Export (index.tsx) — Same for All Types

```tsx
export { default } from './ComponentName';
export type { ComponentNameProps } from './ComponentName';
```
