# repo-conventions

House conventions for `vklyovsa/dev-digest`, extracted from 20 sampled files at `c6af1e4` on 2026-09-24. Flag any change
that violates a rule below and cite the offending `file:line`. Do not flag code
a rule does not cover, and do not restate a rule the diff already follows.

## Structure

- **Constants are placed in a `constants.ts` file co-located with the module that uses them, exported as named constants.**
  Why: Constants files appear 22 times across the repo, always co-located with the module that uses them and exporting named constants.
  Evidence: `client/src/app/repos/[repoId]/pulls/constants.ts:10-17`

```
export const STATUS_META: Record<string, { c: string; labelKey: string }> = {
  needs_review: { c: "var(--warn)", labelKey: "needs_review" },
  reviewed: { c: "var(--ok)", labelKey: "reviewed" },
  stale: { c: "var(--stale)", labelKey: "stale" },
  open: { c: "var(--warn)", labelKey: "open" },
  merged: { c: "var(--ok)", labelKey: "merged" },
  closed: { c: "var(--stale)", labelKey: "closed" },
};
```

- **Component styles are defined in a co-located `styles.ts` file as a const object with `satisfies CSSProperties`.**
  Why: Styles files appear 22 times, each exporting a const object with `satisfies CSSProperties` for type safety.
  Evidence: `client/src/app/repos/[repoId]/pulls/[number]/_components/RunTraceDrawer/styles.ts:4-12`

```
export const s = {
  // ---- TraceSection ----
  section: {
    border: "1px solid var(--border)",
    borderRadius: 8,
    overflow: "hidden",
    marginBottom: 14,
    background: "var(--bg-elevated)",
…
```

## Naming

- **React Query hooks are named `use<Resource>` and use query keys as `["resource"]` for lists and `["resource", id]` for single items, with `enabled: !!id` for dependent queries.**
  Why: All React Query hooks in the client follow the `use<Resource>` naming pattern with query keys as `["resource"]` for lists and `["resource", id]` for single items, and use `enabled: !!id` for dependent queries.
  Evidence: `client/src/lib/hooks/agents.ts:8-13`

```
export function useAgents() {
  return useQuery({
    queryKey: ["agents"],
    queryFn: () => api.get<Agent[]>("/agents"),
  });
}
```

## Imports

- **Shared types are imported from `@devdigest/shared` using `import type` syntax.**
  Why: Shared types are consistently imported from `@devdigest/shared` using `import type` syntax across packages.
  Evidence: `reviewer-core/src/grounding.ts:1`

```
import type { Finding, UnifiedDiff } from '@devdigest/shared';
```

## Error handling

- **Client-side HTTP errors are normalized into `ApiError` with status, code, and details, and thrown from `apiFetch`.**
  Why: Client-side HTTP errors are normalized into `ApiError` with status, code, and details, and thrown from `apiFetch`.
  Evidence: `client/src/lib/api.ts:8-12`

```
export class ApiError extends Error {
  status: number;
  code?: string;
  details?: unknown;
  constructor(message: string, status: number, code?: string, details?: unknown) {
```