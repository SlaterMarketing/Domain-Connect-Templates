# Description

New templates for Kintana (https://kintana.app), a platform for live-event businesses that hosts their websites and sends their email.

- `kintana.app.website`: points a customer's subdomain (e.g. www) at their Kintana website (CNAME to customers.kintana.site, served through Cloudflare for SaaS). `hostRequired: true`.
- `kintana.app.email`: lets Kintana send email from the customer's domain. Groups: `sending` (return-path MX, SPF via SPFM, DKIM), `receiving` (inbound MX, only applied when the customer turns on Kintana's inbox), `dmarc` (`v=DMARC1; p=none;`, only applied when the domain has no DMARC record; `essential: OnApply`).

Both templates are signed (`syncPubKeyDomain: kintana.app`, key at `_dck1.kintana.app`) and pass `dc-template-linter -merge-or-fail`.

## Type of change

- [x] New template
- [ ] Bug fix (non-breaking change which fixes an issue in the template)
- [ ] New feature (non-breaking change which adds functionality to the template)
- [ ] Breaking change (fix or feature that would cause existing template behavior to be not backward compatible)

# How Has This Been Tested?

- [x] Template functionality checked using [Online Editor](https://domainconnect.paulonet.eu/dc/free/templateedit)
- [x] Template file name follows the pattern `<providerId>.<serviceId>.json`
- [x] resource URL provided with `logoUrl` is actually served by a webserver

# Checklist of common problems

- [x] `syncPubKeyDomain` is set
- [x] `warnPhishing` is **not** set alongside `syncPubKeyDomain`
- [x] `syncRedirectDomain` is set whenever the template uses `redirect_uri` in the synchronous flow
- [x] no TXT record contains SPF content (`"v=spf1 ..."`) — SPFM is used
- [x] `txtConflictMatchingMode` is set on every TXT record that must be unique per label or content prefix (DKIM: All; DMARC: Prefix `v=DMARC1`)
- [x] no variable is used as a bare full record value (`p=%dkimkey%`, `feedback-smtp.%region%.amazonses.com`)
- [x] no bare variable is used as the full `host` label (no variables in host at all)
- [x] no variable is used in the `host` field to create a subdomain — the `host` parameter is used
- [x] `%host%` does not appear explicitly in any `host` attribute
- [x] `essential` is set to `OnApply` on records the end user may need to modify or remove (DMARC)

## Online Editor test results

**Editor test link(s):**
- kintana.app.website, host www: [test result](https://domainconnect.paulonet.eu/dc/free/templateedit?token=H4sIAC7Y8GoC%2F91T0W7aMBT9FcuvS4IToKWRJg3RPVQItlKmaapQZOxb8JrYme2QUsS%2Fzw6EFpWHPe8lkX3vOff4%2BHiHLRRlTi3gdIdLrTaCg77jOMXPQloqaUTLEgen0pQWrhWPD0VXMKA3gkEDqWFphKM67R67f572N6CNUBKncYBztVI%2FdO7qa2tLk3Y670Z2BFPSNN%2BwHydRKVcOzsEwLUrbUOCHtaoN2qpKo6MedFSAlER2LQziqqBCRn4y1YIuc7g9o5gq1BZMhOZrQBqY0hzRvKZbg0rliA2iFrHKWFU4%2BVGr0g%2FyzGYr2fdqOYbtbTPtg3e%2BYQZcOGp7qSXgsInOIWtl7Az%2BVA7jjLW6ggAflBmcPu6w3Zbe2NF0OPl6bHfLL%2F6iGsVz5ZaXJbsea53r3StC9ot9gF%2BVhOyNfOFsbkXCC3XpgIip4m1KXddusdKqKjPRQkqqaWF8iFrw4xl60cIfG7yfK1ZSaciM%2B1NbaWjPWVS5FRmt6dsWZ5nzJd86mcZVHctHD%2BQhawd1nFr6Tw4EOOPMyxY%2BwEBukusnTsNkyfthD9h1SHsMwjhZAusO2FU3Sd49hgvv5PJzOHMOjAFpBfXJHzYxw%2Fv9InAu%2FneHcrdu6AZ4Rn1nQpKrMI5DMpgnJI17aXcQ9eP4Jhl8IiQlxB8BjD0cc%2BcS8j4buDuaGNKblvfxbKny5C4RckCeX6D3zcwm952H8asl8ejp%2Bfev4We8%2FwtZ4Vh22QQAAA%3D%3D)
- kintana.app.email, apex, all groups: [test result](https://domainconnect.paulonet.eu/dc/free/templateedit?token=H4sIADTY8GoC%2F%2B1WbW%2FaSBD%2BK6uVortTbcfmJSGukEogF1FKGwLV9S6K0Nq7wArb6%2B6uSdyI%2B%2B03a5sCKYla6aTrSfkC65nZmXlmnhn7AWsWpxHRDPsPOJVixSmTfYp9vOSJJglxSJpi66vqPYnBFA9KJSgUkysesuIKiwmPtrLK9qKSrphUXCTY9ywcibn4KCPQLrROlX98vBPumIciUcWv3fRqTprM4TplKpQ81YUL%2FI5phaoskGIJRUVwNJMiRnrBFaICBAn6dXz1u4V6g%2F4QEbAKRJaEDC3gHPFk%2FptVSEXhlkRRjiQLGV8xxHWhSbMg4mqBCOoNO9ddlIqIh7lj4BDJSRCx3l5eks3h4EMKrEgLYqBSZiHmzB2UKZsRpW3PQXTJ4yXLS%2BMy3V9UmWoRNkSgNaFUnoRXWTBgea%2Bw%2BqY7xuCaUQ7J60MmFmUrZ%2F8KmApJFfZvHvBciiwtOlilDHqdp6Z5w09wXgilK6WhggBHaiJAMmOMBiRc2irWqXNUAj1ySEy%2BQAeZckIRF%2BThQnKdQ%2Btd8Kyh7%2FUT111bz4aGxg2%2FCa7S2XUWMcgb8ySMMsr8x9G%2B1%2F%2Fk02TrXjKjd6ZlG6DuhnFEE1Cl7aOqU0fm8r3uimQG3dFDosMFuBwKavx1oujp6CWtniztm%2F268sTwlB4sK7n7obLSmMjwCdDTjbJCumoXJPdeo7SdiIS9fhbvlWQzfn%2FYpNJtXYIZU1BizYmZ%2Bg9JJ02jfK9et2sLAz423VLzFlLb0JndE9hUrMJeIYBTAXXKN%2FYpkSRWZpttbt7sXb3d3L3B5lzW1jx9HUwjrhpu5MP%2B5WzYcS%2B748%2BX435Q740uzjujj51O4%2FJ9p9c956PB%2BXzU9aog2MDg80RINlXwT3QmoVpaZszCcRZpPiV3ZCui4ZSYUgBqBVqICAO5y5Ck3KIV%2F6tO7Q%2Fedqc8OQuP2TKloakRNxRp0madNsOm7dXpqd0I3dBu0VZg1wLintAZm53VznZeAQfeDodeAtsO7Ta%2BE92RXOG1IekeJQ%2FCXLVh3j10cNLR32R%2F4H5eUM8ulx%2Fl18%2BKeJesb7YA91bZY6Jud9n%2FjqjftTv%2F%2B0ZtFu16fWvBenzZLS%2B75WW3vOyWf3u3wCeTIitGp8TY1dzaie15ttua1Fzfa%2FiNhtNqnjZr3ivX9V3XpM%2BULtE9wPfS7pcSnvejv%2F74c%2FyhPh4sa%2B4VOftSy%2BsT3frcze5P8%2BztuyBQapCOX12M2nj9DzlGCs7CDgAA)
- kintana.app.email, host mail, group sending: [test result](https://domainconnect.paulonet.eu/dc/free/templateedit?token=H4sIADvY8GoC%2F%2B1VYW%2FbNhD9K4SAoBsmKZJtuZkKA5PtNsgyt7aTbtkCw6Cks01EElWScqIG6W%2FfUVJiO7GL9Vsx9Ist3B3f3bt7PN4bCtI8oQoM%2F97IBV%2BzGMRZbPjGDcsUzahN89wwn1zvaYqhxnntRIcEsWYRVEcgpSzZ2JrYt411DUIynhm%2BaxoJX%2FKPIkHvSqlc%2BsfHW%2BmOWcQzWf1antuy82yJx2OQkWC5qiCMP0BJ0lRBJGQxqZKTheApUSsmSczRkJGfLsbvTDI8PxsRilEhL7IIyAq%2FE5YtfzYrK69gaZKUREAEbA2EqcqTF2HC5IpQMhwF0wHJecKi0tZ0qGA0TGC4U5eAJX74WAJUZWEOUttMAvbSJoW0gEpluTaJb1h6A2UdXJf7StalVmkjgl6dSpZZNC7CcyiHVdSL6eiAKcQMi1f7QswY1vbuEQzlIpaGf31vLAUv8mqCTcnoV2Wuhze6wu8Vl6pxailwBJKXHC0LgDik0Y0lU5XbRzXRI5um9DNOEKQd8bQSD%2BOCqRJH7yCywrm3u47zYH41NQ5u9CK5zBfTIgGs22BZlBQx%2BM%2Bz%2FVf8y6vLDbwA7bfn9Riw71pxVFF05b2jZlJH%2BvCdGvBsgdNRI6qiFUKOeKzxgiQ5nL2W1cHW%2FrbbV5ZpncZ720pvv6mtcUpFdID0%2FNHZMF33KpG7b0jey3gGb77Kdyxgwe72hzS%2BDSSGgcQWK0b1rf%2BQBXmelDv9mj2YBvKD%2BUaaMyztUc5wR3FTQcO9YdCslorunFVnngaNh3MqaCr1anuEud7BmT0CXddIswbqGUw9gOpwYd2Cvr3a3KhC20dnp4tR4JwOLj6dXpyF7eHkbT%2BYfAyCzun7YDjos8l5fzkZtJrkhubKlhkXMJf4T1UhsKVKFGAaaZEoNqe3dGOKoznV%2FcLWSPRiRry12zLK6lVbabjpSTPT3Sv6VL998NY819U8jnQDmRZTp73wXnteZLU7i67V6YaedeK1Q6vr0JiG4J20Wt2tx2LPO7Lvudid5bZMguSWltJ40JLeEfBhvusergiX7F0O5AvdvaPfN7sXO%2BkZ1bz3rbL7%2FqjPTLzkP8T8Q8z%2FCzHjoyDpGuI51aEtp9W1XNdyTi5bju92fM%2B1vc7rVtv7xXF8x9EMUL91A%2B7xRdh%2BC4zU%2B7X1YepOrn5fSin%2B6kz7Khy8%2BzNTZd9NguN%2FePA5Hi%2FicbD8u2c8%2FAvtus%2FvyQsAAA%3D%3D)
