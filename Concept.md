# Nx টুল কী? 

Nx হলো একটি **monorepo build system ও tooling**। একটি Git repository-তে একাধিক অ্যাপ ও লাইব্রেরি রাখা যায়, আর Nx সেগুলোকে দ্রুত ও গোছানোভাবে build, test ও run করতে সাহায্য করে।

Microservice architecture-এ এটি খুব কাজে লাগে।

---

## সমস্যাটা কোথায়?

Microservice বানালে অনেকগুলো প্রজেক্ট তৈরি হয়:

```
auth-service/
order-service/
product-service/
api-gateway/
```

প্রতিটি যদি আলাদা repository হয়, তাহলে:

- Shared code (DTO, type, auth guard) কপি-পেস্ট করতে হয়
- এক জায়গায় পরিবর্তন করলে অন্য জায়গায় হাতে হাতে আপডেট করতে হয়
- প্রতিটি repo-তে আলাদা dependency ও আলাদা config সামলাতে হয়

---

## Nx কীভাবে সমাধান করে? (Monorepo)

সব service **একটি repository-তে** থাকে, কিন্তু প্রতিটি আলাদা অ্যাপ হিসেবে:

```
my-workspace/
 ├── apps/
 │    ├── auth-service/
 │    ├── order-service/
 │    ├── product-service/
 │    └── api-gateway/
 ├── libs/
 │    ├── shared-dto/        (সব service ব্যবহার করে)
 │    ├── shared-auth/       (JWT guard, decorator)
 │    └── shared-types/
 ├── nx.json
 └── package.json
```

`libs/` ফোল্ডারের কোড একবার লিখলে সব অ্যাপ import করতে পারে:

```ts
import { CreateOrderDto } from '@my-workspace/shared-dto';
```

---

## Nx-এর প্রধান ফিচার

| ফিচার | কী করে |
|---|---|
| **Affected commands** | শুধু যেটা পরিবর্তন হয়েছে সেটাই build/test করে। `auth-service` বদলালে `order-service` আবার build হয় না (যদি dependency না থাকে) |
| **Caching** | একবার build করলে ফলাফল cache হয়। একই কোড হলে পরেরবার প্রায় সাথে সাথেই শেষ হয় |
| **Dependency graph** | `nx graph` দিয়ে দেখা যায় কোন অ্যাপ কোন লাইব্রেরির উপর নির্ভর করে |
| **Generators** | `nx g @nx/nest:app order-service` দিয়ে নতুন অ্যাপ/লাইব্রেরি অটো-জেনারেট হয় |
| **CI/CD friendly** | `nx affected -t build` দিয়ে Jenkins/GitHub Actions-এ শুধু পরিবর্তিত service build করা যায় |

---

## কিছু কমন কমান্ড

```bash
# নতুন workspace তৈরি
npx create-nx-workspace@latest

# NestJS অ্যাপ যোগ করা
nx g @nx/nest:app order-service

# Shared library বানানো
nx g @nx/js:lib shared-dto

# একটি অ্যাপ চালানো
nx serve order-service

# সব অ্যাপ build করা
nx run-many -t build

# শুধু পরিবর্তিত অ্যাপগুলোর test
nx affected -t test

# Dependency graph দেখা
nx graph
```

---

## Prisma-র সাথে কীভাবে কাজ করে?

Microservice-এ প্রতিটি service-এর নিজস্ব Prisma schema থাকে। Nx-এও একই নিয়ম:

```
apps/
 ├── auth-service/prisma/schema.prisma
 └── order-service/prisma/schema.prisma
```

প্রতিটি অ্যাপ নিজের Prisma client generate করে এবং আলাদা database-এ connect করে। Nx শুধু structure ও build ম্যানেজ করে, database আলাদা রাখার নিয়ম বদলায় না।

---

## কখন Nx লাগবে?

**লাগবে যদি:**

- ৩টি বা তার বেশি service/অ্যাপ থাকে এবং shared code বেশি
- টিমে অনেক মানুষ কাজ করে
- CI সময় কমাতে চাও (affected + cache দিয়ে)

**দরকার নেই যদি:**

- একটিই NestJS monolith (যেমন তোমার ecommerce বা news portal এখন)
- Solo developer এবং ছোট প্রজেক্ট

তোমার বর্তমান প্রজেক্টগুলো modular monolith হিসেবে ভালোই চলবে। যখন microservice-এ যাবে, তখন Nx ব্যবহার করা বুদ্ধিমানের কাজ হবে, কারণ তখন `shared-dto`, `shared-auth`-এর মতো লাইব্রেরিগুলো অনেক কাজে আসবে।

---

## Nx বনাম Turborepo

দুটোই monorepo tool।

| | Nx | Turborepo |
|---|---|---|
| ফিচার | বেশি (generator, graph, plugin) | কম, হালকা |
| মূল ফোকাস | পূর্ণাঙ্গ monorepo tooling | মূলত caching |
| NestJS microservice | বেশি জনপ্রিয় | সরল প্রজেক্টে ভালো |

---

## সারসংক্ষেপ

- Nx = একটি repo-তে সব service রাখার ও সেগুলো দ্রুত build/test করার টুল
- Shared code `libs/`-এ রেখে সব অ্যাপে ব্যবহার করা যায়
- `affected` ও caching CI সময় অনেক কমায়
- ছোট monolith-এ দরকার নেই, microservice-এ গেলে কাজের
