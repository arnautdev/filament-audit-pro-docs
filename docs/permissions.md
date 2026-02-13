# Permissions

By default, the Activity Log resource is **read-only**:
- No create actions
- No edit actions
- View page only

How you restrict access depends on your Filament setup.

---

## Option A: Filament Shield (recommended)

If you use Filament Shield, generate permissions and assign them to roles.

Typical permissions:
- `view_any_activity_log`
- `view_activity_log`

(Exact names depend on your resource policy / Shield configuration.)

---

## Option B: Resource authorization

You can restrict access by implementing authorization in your app:

### 1) Register a Policy for the Activity model

Example policy methods:
- `viewAny`
- `view`

Then ensure Filament is using it.

---

## Option C: Panel-level access

If your panel is only accessible to admins, that may be enough.

---

## Notes

- This plugin does not force a permission system.
- The recommended approach is to use Filament Shield or your own policies.
