# AURORA CLI

A small command-line tool to request molecule generation from the AURORA API — the same service used by the web portal.

**File Name:** `AURORA_CLI.py`

---

## Setup (one time)

```bash
pip install requests
```

Get an **API key** from the portal: '(https://aurora.raylab.iiitd.edu.in/)`

```bash
export MOLGEN_API_KEY="paste-your-api-key-here"
```

---

## Basic usage

You must provide **two SMILES**, a **database**, and a **Tanimoto threshold**:

```bash
python AURORA_CLI.py \
  --smiles1 "CCO" \
  --smiles2 "CCN" \
  --database zinc \
  --tanimoto 0.8
```

Results are saved as `molecule_results.zip` in the current folder.

---

## Common options


| Option            | Meaning                                                 |
| ----------------- | ------------------------------------------------------- |
| `--smiles1`       | First SMILES (required)                                 |
| `--smiles2`       | Second SMILES (required)                                |
| `--database`      | `zinc` or `chembl` (required)                           |
| `--tanimoto`      | Number from **0.0** to **1.0** (required)               |
| `--api-key`       | Your API key (or use `MOLGEN_API_KEY`)                  |
| `-o` / `--output` | Where to save the ZIP (default: `molecule_results.zip`) |
| `--extract`       | Unzip the results after download                        |
| `--health-check`  | Test the server before starting a job                   |


### Example with extra options

```bash
python AURORA_CLI.py \
  --smiles1 "c1ccccc1" \
  --smiles2 "c1ccncc1" \
  --database chembl \
  --tanimoto 0.75 \
  --output my_results.zip \
  --extract
```

### Optional advanced flags

Only use these if you know you need them:

`--beam-size`, `--epsilon`, `--rank-cutoff`, `--max-steps`, `--sim-threshold`, `--alpha`, `--beta`, `--gamma`

---

## Need help?

Run the script with **no arguments** for a short introduction:

```bash
python AURORA_CLI.py
```

For **full documentation** of every flag:

```bash
python AURORA_CLI.py --help
```

---

## Important notes

1. **Jobs can take a long time** — the script waits until the server finishes (minutes to hours).
2. **Only one job at a time** on the server — if you see “Another job is already running”, wait or ask your admin.
3. **Default API address:** `http://192.168.27.115:8020`
  Override with `--base-url` or `MOLGEN_BASE_URL`.

---

## Running many SMILES pairs

Call this script once per pair from another script (bash or Python), **one after another**. Use a different `--output` for each pair.

Example:

```bash
python AURORA_CLI.py --smiles1 "CCO" --smiles2 "CCN" --database zinc --tanimoto 0.8 -o pair_1.zip
python AURORA_CLI.py --smiles1 "CCN" --smiles2 "CCO" --database zinc --tanimoto 0.8 -o pair_2.zip
```

---

## Troubleshooting


| Problem            | What to try                                              |
| ------------------ | -------------------------------------------------------- |
| Invalid API key    | Create a new key on the website                          |
| Connection error   | Check server is up (see `DEPLOYMENT.md` in project root) |
| HTTP 429           | Another job is running — wait and retry                  |
| Unsure about flags | `python AURORA_CLI.py --help`                           |


