# Terraform Environment Variables and `export`

When working with Terraform, environment variables prefixed with `TF_VAR_` are automatically picked up as input variables. However, it’s important to understand how shell variables and exported environment variables behave.

---

## Problem

If you define a variable in your `.env` file like this:

```sh
# .env
TF_VAR_eks_admin_principal_arn="arn:aws:iam::183056140671:user/devninja"
```

and then run:

```sh
source .env
```

Terraform **will not detect** the variable.

---

### Why?

* ✅ Running `source .env` sets the **shell variable** `TF_VAR_eks_admin_principal_arn` in your current session.
* ❌ But it does **not export** it to the environment of child processes (such as Terraform, AWS CLI, or scripts).

Since Terraform runs as a **child process**, it only sees exported environment variables, not just shell-local ones.

---

## Solution

Use `export` to ensure the variable is passed to child processes:

```sh
export TF_VAR_eks_admin_principal_arn="arn:aws:iam::183056140671:user/devninja"
```

This tells the shell:

> "Make this variable available to all child processes."

---

## Alternative: Export Inside `.env`

You can also export directly inside your `.env` file:

```sh
# .env
export TF_VAR_eks_admin_principal_arn="arn:aws:iam::183056140671:user/devninja"
```

Then, when you run:

```sh
source .env
```

Terraform will detect the variable because it’s now in the environment, not just the shell session.

---

## Key Takeaway

* Shell variables (`VAR=value`) are **local to the shell session**.
* Exported variables (`export VAR=value`) are **inherited by child processes**.
* Terraform automatically picks up any environment variables with the `TF_VAR_` prefix.

---
