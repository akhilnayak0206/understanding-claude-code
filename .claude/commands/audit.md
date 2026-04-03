# Update Vulnerable Dependencies

## Steps to Update Vulnerable Dependencies

1. **Run npm audit** to check for vulnerabilities
   ```bash
   npm audit
   ```

2. **Run npm audit fix** to automatically fix vulnerabilities
   ```bash
   npm audit fix
   ```

3. **Verify updates by running tests**
   ```bash
   npm test
   ```

## Notes

- Check the audit report to understand the severity of vulnerabilities
- Some vulnerabilities may require manual updates if they can't be fixed automatically
- Always test your application after updating dependencies to ensure compatibility