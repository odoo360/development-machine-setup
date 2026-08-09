
# Extracting & Updating `fa.po` Files from Odoo Docker Container

This guide explains how to copy all Persian (`fa.po`) translation files out of an
Odoo Docker container, edit them on your Windows host, and push the updated
files back into the container. Commands are written for **PowerShell**.

---

## 0. Prerequisites

- Docker Desktop running, with the Odoo container up.
- PowerShell (commands below assume `docker` is on your PATH).

---

## 1. Find your container ID/name

```powershell
docker ps
```

Copy the container ID or name (e.g. `d9f159c91137`) and store it in a variable
so you don't have to retype it:

```powershell
$container = "d9f159c91137"
```

---

## 2. Locate all `fa.po` files inside the container

```powershell
docker exec $container bash -c "find / -iname fa.po 2>/dev/null"
```

This lists every `fa.po` file across core and custom addons — typically under:

```
/usr/lib/python3/dist-packages/odoo/addons/<module>/i18n/fa.po
```

Optional: save the list to a file inside the container for reference:

```powershell
docker exec $container bash -c "find / -iname fa.po 2>/dev/null > /tmp/fa_po_list.txt"
docker exec $container wc -l /tmp/fa_po_list.txt
```

---

## 3. Copy files one by one (no `tar` needed)

This container doesn't have `tar` installed, so files are copied individually
via `docker cp`:

```powershell
$files = docker exec $container bash -c "find / -iname fa.po 2>/dev/null"

foreach ($f in $files) {
    $dest = ".\fa_po_edit" + ($f -replace '/', '\')
    $destDir = Split-Path $dest
    New-Item -ItemType Directory -Force -Path $destDir | Out-Null
    docker cp "${container}:$f" $dest
}
```

This loops through every path found in Step 2 and copies each file
individually into `.\fa_po_edit`, recreating the same directory structure
(e.g. `.\fa_po_edit\usr\lib\python3\dist-packages\odoo\addons\<module>\i18n\fa.po`).

---

## Notes / Gotchas

- **PowerShell angle brackets:** Never type `<container_name>` literally —
  PowerShell reserves `<` and `>` for redirection. Always substitute the real
  container ID/name, or use the `$container` variable shown above.
- **`docker cp` with variables:** Use `"${container}:/path"` syntax (curly
  braces) so PowerShell doesn't misparse the variable name against the colon.
- **Verify each step:** After copying, check the file actually landed before
  moving to the next step:
  ```powershell
  Get-ChildItem .\fa_po_files.tar
  ```