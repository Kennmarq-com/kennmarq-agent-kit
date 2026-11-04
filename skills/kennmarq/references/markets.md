# Markets: what each call must send

`markets` names the countries the app serves. Each file is then marked for their laws and gets a receipt
(`GET /v1/marks/{id}/receipt`). A call missing what a market needs is refused before anything is counted, with a
message naming the missing field, so a mistake shows up while you build.

| Market | Law | The call must send | Visible label |
| --- | --- | --- | --- |
| `eu` | EU AI Act, Article 50 | nothing more; `provider` and `app_name` are recommended | on deep fakes (`deepfake: true`) |
| an EU member state (`de`, `it`…) | the AI Act, plus that state's own rules | what its own rules add | on deep fakes, or where its rules need one |
| `us-ca` | California AI Transparency Act | `provider`, `app_name`, `app_version` | no |
| `us-wa` | Washington HB 1170 (from 1 February 2027) | `provider`, `app_name`, `app_version` | no |
| `us-ut` | Utah HB 276 (from 1 January 2027) | nothing more | no |
| `us` | every US state above | `provider`, `app_name`, `app_version` | no |
| `kr` | South Korea, AI Basic Act | nothing more | on deep fakes |
| `in` | India, IT Rules amendment of 2026 | `app_name` | on every image and video |
| any other ISO code (`jp`, `gb`, `br`…) | that country's rules, from the world law map | what its rules need | where its rules need one |

Refused until Kennmarq.com adds what their rules need, with a message saying so:
- `cn` (China): its metadata format (GB 45438-2025);
- deep-fake audio for `eu` and `kr`, and any audio for `in`: they need a spoken notice;
- `global`: name the markets instead, e.g. `eu,us,kr,in,jp`.

When unsure which markets an app serves, ask the user. Each country's rules, with links to the government texts:
https://kennmarq.com/docs/laws
