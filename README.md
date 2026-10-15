## Nx Monorepo Setup

#### Nx Install
```bash
npm i -g nx
```
> version check
```bash
nx --version
```
> or,
```bash
npx nx --version
```
---

#### Notun Nx Workspace toiry koro
```bash
npx create-nx-workspace@latest my-workspace
```
<img width="376" height="63" alt="image" src="https://github.com/user-attachments/assets/cbba107b-4c1b-4e68-88b9-cbc0f42431da" />

<img width="717" height="253" alt="image" src="https://github.com/user-attachments/assets/47c295e2-236c-4171-bb0a-e33d60fec7f2" />

<img width="955" height="191" alt="image" src="https://github.com/user-attachments/assets/c14df0e3-f545-4959-be9e-ac2e2651e686" />

<img width="538" height="131" alt="image" src="https://github.com/user-attachments/assets/1c77660f-6425-4b23-8f7e-afde5b945807" />

```bash
cd my-workspace
```
```bash
code .
```
---

#### Nx Workspace-e NestJS Plugin Install kora
```bash
npm i --save-dev @nx/nest
```
---

#### NestJS app add kora [service create koro]
```bash
npx nx g @nx/nest:app auth-service
```
<img width="497" height="223" alt="image" src="https://github.com/user-attachments/assets/1711e9e5-ec94-405e-8f69-706645c1505f" />

<img width="1099" height="332" alt="image" src="https://github.com/user-attachments/assets/c38c9261-480a-41ad-810e-ea54fee36d58" />

```bash
npx nx g @nx/nest:app order-service
```
---

#### Shared library banano
```bash
npx nx g @nx/js:lib shared-dto
```
<img width="913" height="277" alt="image" src="https://github.com/user-attachments/assets/84f36c88-d0c4-472e-afbf-30c85417df75" />

<img width="926" height="275" alt="image" src="https://github.com/user-attachments/assets/d819f32f-8c86-4710-91ba-99269c5c0182" />

<img width="924" height="339" alt="image" src="https://github.com/user-attachments/assets/aec226d3-d036-40a5-b9fd-81c267206ec7" />

---

#### Ekta app run kora [service run koro]
```bash
npx nx serve order-service
```
> OUTPUT
<img width="1377" height="310" alt="image" src="https://github.com/user-attachments/assets/071dca46-a2c0-4167-ba14-f1ab9e16b634" />

<img width="431" height="210" alt="image" src="https://github.com/user-attachments/assets/bc073549-9ada-4c66-947f-e152a66f91e6" />

---
