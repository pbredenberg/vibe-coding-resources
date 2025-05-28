- always write backend services in the same way. here's an example service class:

```typescript
import { supabase } from '@game/lib/server/services/SupabaseService';

class ExampleService {
  constructor(private readonly _supabase = supabase) {}
}

export const exampleService = new ExampleService();
```

- the above uses dependency injection, where dependencies are private, readonly, with a underscore for a prefix
- follow OOP principles
- keep functions small, favor composabilty
- when writing supabase queries, check for errors, but simply log them when present, and continue to return a sensible default at the end of the function. example:

```typescript
  async getUserExampleData(id: string) {
    const { data, error } = await this._supabase
      .from('examples')
      .select('*')
      .eq('id', id)
      .single();

    if (error) {
      console.error('Error getting example data:', error);
    }

    return data; // sensible default could include an empty array: return data ?? []
  }
```
