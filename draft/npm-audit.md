➜  react-sample git:(master) npm i -D babel-loader@7.1.4 style-loader@0.20.3 css-loader@0.28.11 babel-core@6.26.0 babel-preset-env@1.6.1 babel-preset-react@6.24.1
npm WARN react-sample@1.0.0 No description
npm WARN react-sample@1.0.0 No repository field.

+ style-loader@0.20.3
+ babel-preset-react@6.24.1
+ css-loader@0.28.11
+ babel-loader@7.1.4
+ babel-core@6.26.0
+ babel-preset-env@1.6.1
added 128 packages from 205 contributors and updated 1 package in 23.466s
[!] 1 vulnerability found [15197 packages audited]
    Severity: 1 Critical
    Run `npm audit` for more detail

➜  react-sample git:(master) ✗ npm audit

                       === npm audit security report ===

┌──────────────────────────────────────────────────────────────────────────────┐
│                                Manual Review                                 │
│            Some vulnerabilities require your attention to resolve            │
│                                                                              │
│         Visit https://go.npm.me/audit-guide for additional guidance          │
└──────────────────────────────────────────────────────────────────────────────┘
┌───────────────┬──────────────────────────────────────────────────────────────┐
│ Critical      │ Command Injection                                            │
├───────────────┼──────────────────────────────────────────────────────────────┤
│ Package       │ macaddress                                                   │
├───────────────┼──────────────────────────────────────────────────────────────┤
│ Patched in    │ No patch available                                           │
├───────────────┼──────────────────────────────────────────────────────────────┤
│ Dependency of │ css-loader [dev]                                             │
├───────────────┼──────────────────────────────────────────────────────────────┤
│ Path          │ css-loader > cssnano > postcss-filter-plugins > uniqid >     │
│               │ macaddress                                                   │
├───────────────┼──────────────────────────────────────────────────────────────┤
│ More info     │ https://nodesecurity.io/advisories/654                       │
└───────────────┴──────────────────────────────────────────────────────────────┘

[!] 1 vulnerability found - Packages audited: 15197 (15117 dev, 97 optional)
    Severity: 1 Critical

➜  react-sample git:(master) ✗