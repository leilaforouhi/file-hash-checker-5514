#!/usr/bin/env python3
"""
File Hash Checker
- Computes SHA256 hash for each file in the repo
- Saves results into hashes.txt
"""

import os, hashlib, datetime

def sha256_of_file(path):
    h = hashlib.sha256()
    try:
        with open(path, "rb") as f:
            for chunk in iter(lambda: f.read(8192), b""):
                h.update(chunk)
        return h.hexdigest()
    except Exception:
        return None

def main():
    lines = []
    timestamp = datetime.datetime.now().isoformat(timespec="seconds")
    lines.append(f"# Hash report generated at {timestamp}\n")

    for root, _, files in os.walk("."):
        if ".git" in root:
            continue
        for name in files:
            path = os.path.join(root, name)
            if path.endswith(".py") or path.endswith(".txt") or path.endswith(".md"):
                digest = sha256_of_file(path)
                if digest:
                    lines.append(f"{digest}  {path}")

    with open("hashes.txt", "w", encoding="utf-8") as f:
        f.write("\n".join(lines))

    print("Hash report saved to hashes.txt")

if __name__ == "__main__":
    main()
