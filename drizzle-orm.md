## Types Retrieval

### InferSelectModel - get names and types of all columns of table

```ts
import { type InferSelectModel } from "drizzle-orm";
import type { bank_account } from "@/server/db/drizzle/schema.ts/schema";
export type details1 = {
  bankDetails: InferSelectModel<typeof bank_account>;
};
```

### GetColumnData - get type of column

- https://github.com/drizzle-team/drizzle-orm/blob/main/drizzle-orm/src/column.ts#L138

```ts
import type { GetColumnData } from "drizzle-orm";
import type { beneficiary } from "@/server/db/drizzle/schema.ts/schema";

export type Benificiary1 = {
  id: number;
  name: string | null;
  status: GetColumnData<typeof beneficiary.status>;
}[];
```
