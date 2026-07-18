# 🚀 چرا نباید Business Logic را داخل `app/api/.../route.ts` بنویسیم؟

در پروژه‌های **Next.js**، فایل `route.ts` باید فقط نقش یک **Adapter** یا **Controller** را داشته باشد؛ یعنی:

- درخواست (Request) را دریافت کند.
- داده را به Service ارسال کند.
- پاسخ (Response) را برگرداند.

نباید منطق اصلی برنامه داخل این فایل نوشته شود.

---

# ❌ اشتباه رایج

قرار دادن تمام منطق برنامه داخل `app/api/.../route.ts`

مانند:

- Validation
- Business Logic
- Database Query
- Audit Log
- Notification
- AI Logic
- Authorization Logic

نمونه:

```ts
// app/api/crm/leads/route.ts

export async function POST(req: Request) {

    const body = await req.json();

    if (!body.name)
        return Response.json(
            { error: "Name required" },
            { status: 400 }
        );

    const lead = await prisma.lead.create({
        data: body
    });

    await prisma.auditLog.create({
        data: {
            action: "CREATE_LEAD"
        }
    });

    return Response.json(lead);
}
```

### مشکل این روش

- وابستگی شدید به Next.js
- تست‌نویسی سخت
- استفاده مجدد از Business Logic تقریباً غیرممکن
- مهاجرت به NestJS یا هر Backend دیگر پرهزینه می‌شود.

---

# ✅ روش پیشنهادی

ساختار پروژه:

```text
app/
 └── api/
      └── crm/
           └── leads/
                route.ts

src/
 └── modules/
      └── crm/
           ├── services/
           ├── repositories/
           ├── validators/
           ├── dto/
           └── entities/
```

---

## route.ts

```ts
import { LeadService } from "@/src/modules/crm/services/lead.service";

export async function POST(req: Request) {

    const body = await req.json();

    const lead = await LeadService.create(body);

    return Response.json(lead);

}
```

همین!

Route فقط Request را می‌گیرد و Response را برمی‌گرداند.

---

## Service

تمام Business Logic اینجاست.

```ts
export class LeadService {

    static async create(data) {

        if (!data.name)
            throw new Error("Name required");

        const lead = await LeadRepository.create(data);

        // Audit Log

        // Notification

        // Queue

        // AI

        return lead;

    }

}
```

اینجاست که تصمیمات برنامه گرفته می‌شود.

---

## Repository

فقط ارتباط با دیتابیس.

```ts
export class LeadRepository {

    static create(data) {

        return prisma.lead.create({
            data
        });

    }

}
```

Repository نباید Business Logic داشته باشد.

---

# مزایا

✅ Separation of Concerns

هر لایه فقط یک مسئولیت دارد.

---

✅ کد تمیزتر

خواندن و نگهداری پروژه بسیار ساده‌تر می‌شود.

---

✅ تست‌پذیری بهتر

می‌توان Service را بدون اجرای Next.js تست کرد.

---

✅ استفاده مجدد

همان Service را می‌توان در:

- API
- Cron Job
- Queue
- AI Agent
- CLI

استفاده کرد.

---

✅ مهاجرت آسان به NestJS

فرض کنید دو سال بعد تصمیم بگیرید از Next API به NestJS مهاجرت کنید.

الان:

```text
Next Route
      │
      ▼
LeadService
      │
      ▼
LeadRepository
```

بعداً:

```text
Nest Controller
      │
      ▼
LeadService
      │
      ▼
LeadRepository
```

تقریباً هیچ تغییری در Business Logic ایجاد نمی‌شود.

---

# معماری پیشنهادی برای پروژه‌های متوسط

```text
app/
    api/

src/
    modules/

        crm/
            services/
            repositories/
            validators/
            dto/
            entities/
            events/

        kanban/
            ...

        lms/
            ...

        ai/
            ...

        notification/
            ...

lib/
db/
```

در این معماری:

- `app/api` فقط نقش Adapter دارد.
- تمام منطق برنامه داخل `src/modules` قرار می‌گیرد.

---

# آیا از همین ابتدا باید NestJS بسازیم؟

خیر.

اگر پروژه هنوز در حال رشد است، می‌توان با Next.js ادامه داد.

اما از همان روز اول باید Business Logic را از Route Handlerها جدا کرد.

این کار باعث می‌شود اگر پروژه بزرگ شد، بتوان تقریباً بدون بازنویسی کدها به NestJS مهاجرت کرد.

---

# قانون ساده‌ای که همیشه باید رعایت کنیم

```
Route
    ↓
Service
    ↓
Repository
    ↓
Database
```

---

# خلاصه

**Route**

- دریافت Request
- ارسال Response

---

**Service**

- Business Logic
- Validation
- Authorization
- Workflow
- AI
- Notification
- Queue

---

**Repository**

- فقط ارتباط با دیتابیس

---

> **نکته مهم:**  
> اگر امروز پروژه با Next.js توسعه داده می‌شود، لزوماً نیازی به ساخت Backend جداگانه نیست. اما اگر از ابتدا Business Logic را از Route Handlerها جدا کنید، در آینده مهاجرت به NestJS یا هر معماری دیگری بسیار ساده خواهد بود و نیازی به بازنویسی منطق برنامه نخواهید داشت.
