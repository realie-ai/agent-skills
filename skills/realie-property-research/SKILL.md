---
name: realie-property-research
description: Research U.S. properties with the Realie MCP tools. Use when the user asks about a specific address or parcel/APN, wants property details (owner, sales history, mortgages, liens, tax assessment, building facts, AVM value), wants to find properties matching filters or near a location, or wants comparable sales to value a property.
---

# Realie property research

The Realie MCP server returns live U.S. parcel records. Every successful tool call uses one Realie token, so pick the most specific tool and keep page sizes small.

## Pick the tool

| Need | Tool | Notes |
| --- | --- | --- |
| One property by street address | `address_lookup` | `address` + `state` required. If you pass `city`, also pass `county`. |
| One property by APN or Realie ID | `parcel_lookup` | APN needs `state` + `county`. A `realieParcelId` (`{county FIPS}-{APN}`, e.g. `06075-3720-009`) works alone. |
| Properties matching filters | `property_search` | `state` required; add `county`, `city`, `zipCode`, `address`, `useCode`, `foreclosure`, or `transferedSince`. |
| Properties near a point | `location_search` | `latitude`, `longitude`, `radius` in miles (capped at 2). |
| Comparable sales / valuation | `comparables` | Subject `latitude`/`longitude`; filter by beds, baths, sqft, price, `propertyType`, `timeFrame` (months). |
| Properties held by a named owner | `owner_search` | Business research only (see Limits). |

## Workflow

1. Resolve the subject first. Use `address_lookup` (or `parcel_lookup` for an APN) and keep the returned `realieParcelId`, `latitude`, and `longitude`.
2. Reuse what you have. Re-fetch the same parcel with `parcel_lookup` and its `realieParcelId`; pass the subject's coordinates straight to `comparables` or `location_search`.
3. Page, don't widen. List tools return at most 10 records per call (`limit` / `maxResults` 1–10). Continue with `cursor` (search) or `offset` (location, owner) only when the user needs more.
4. Report the fields the user asked for and cite the parcel (address + `realieParcelId`). Say plainly when a field is empty; some parcels (e.g. government-owned) have no assessed value.

## Errors

- **No record found (404):** not billed. Check the address spelling, add or fix `county`, or try `parcel_lookup`.
- **401 / sign-in prompt:** the user needs to connect their Realie account (OAuth). No API key goes in the config.
- **Tool error with a `code` and `action`:** the account can't make API calls yet (for example, no payment method). Relay the `action` text to the user as-is.

## Limits

- Realie is not a consumer reporting agency. Do not use Realie data to decide a person's eligibility for credit, insurance, employment, or housing (including tenant screening).
- Use `owner_search` for business and property research, not to profile individuals.
