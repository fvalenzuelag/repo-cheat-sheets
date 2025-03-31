# John the Ripper Cheat Sheet

## Basic Usage Commands

### Starting a Basic Attack
```bash
# Basic syntax
john [options] [path/to/password/file]

# Basic hash cracking
john hash.txt

# Show cracked passwords
john --show hash.txt

# Show crack status
john --status
```

## Common Attack Modes

### Dictionary Attack
```bash
# Using a wordlist
john --wordlist=/path/to/wordlist hash.txt

# Using rules with wordlist
john --wordlist=/path/to/wordlist --rules hash.txt
```

### Incremental Mode (Brute Force)
```bash
# Full brute force
john --incremental hash.txt

# Specific character set
john --incremental:ASCII hash.txt
```

## Format Options

### Specify Hash Format
```bash
# List available formats
john --list=formats

# Use specific format
john --format=md5crypt hash.txt
john --format=sha512crypt hash.txt
john --format=nt hash.txt
```

## Performance Options

### Optimization
```bash
# Set number of threads
john --threads=4 hash.txt

# Fork multiple processes
john --fork=4 hash.txt
```

## Session Management

### Managing Sessions
```bash
# Save session
john --session=mysession hash.txt

# Restore session
john --restore=mysession

# Stop current session
ctrl+c
```

## Advanced Options

### Password Policy Rules
```bash
# Custom rules
john --wordlist=wordlist.txt --rules=custom hash.txt

# Show current rules
john --list=rules
```

### Custom Character Sets
```bash
# Define character set
john --incremental="MyCharSet" --charset=alpha hash.txt

# Show available charsets
john --list=chr
```

## Hash Preparation

### Converting Hash Formats
```bash
# Convert various formats to John format
john2zip zipfile.zip > hash.txt
office2john document.docx > hash.txt
ssh2john id_rsa > hash.txt
```

## Best Practices

1. Always create backups of hash files before processing
2. Start with fastest methods first (dictionary) before moving to slower ones
3. Use appropriate rules based on target password policy
4. Monitor system resources during long runs
5. Document successful patterns for future reference

## Configuration File Location
- Unix-like: ~/.john/john.conf
- Windows: john.conf in installation directory

Remember to follow ethical guidelines and obtain proper authorization before using these techniques.