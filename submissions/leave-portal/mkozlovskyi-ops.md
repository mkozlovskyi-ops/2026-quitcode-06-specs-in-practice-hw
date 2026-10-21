# Здача WS6

## Дані

- Роль: pm
- Команда: leave-portal
- GitHub-логін: mkozlovskyi-ops
- Ваше репо: https://github.com/mkozlovskyi-ops/course-staff-management-spec
- SHA: 3ec0666b22774b4ab34032fb79380932865b06f3
- Репо вимог PM: https://github.com/mkozlovskyi-ops/course-staff-management-spec
- Прод-URL: —

## PM

- Сховище вимог: docs/
- Продукт: https://github.com/mkozlovskyi-ops/course-staff-management-spec/blob/3ec0666b22774b4ab34032fb79380932865b06f3/docs/product.md
- Вимоги першого зрізу: https://github.com/mkozlovskyi-ops/course-staff-management-spec/blob/3ec0666b22774b4ab34032fb79380932865b06f3/docs/requirements/01-leave-request.md, https://github.com/mkozlovskyi-ops/course-staff-management-spec/blob/3ec0666b22774b4ab34032fb79380932865b06f3/docs/requirements/02-leave-decision.md, https://github.com/mkozlovskyi-ops/course-staff-management-spec/blob/3ec0666b22774b4ab34032fb79380932865b06f3/docs/requirements/03-leave-balance.md
- Самоперевірка за чеклістом: https://github.com/mkozlovskyi-ops/course-staff-management-spec/blob/3ec0666b22774b4ab34032fb79380932865b06f3/docs/requirements/01-leave-request.md#L106, https://github.com/mkozlovskyi-ops/course-staff-management-spec/blob/3ec0666b22774b4ab34032fb79380932865b06f3/docs/requirements/02-leave-decision.md#L108, https://github.com/mkozlovskyi-ops/course-staff-management-spec/blob/3ec0666b22774b4ab34032fb79380932865b06f3/docs/requirements/03-leave-balance.md#L110
- Питання, на які ви відповіли: https://github.com/mkozlovskyi-ops/course-staff-management-spec/issues/1, https://github.com/mkozlovskyi-ops/course-staff-management-spec/issues/2, https://github.com/mkozlovskyi-ops/course-staff-management-spec/issues/3, https://github.com/mkozlovskyi-ops/course-staff-management-spec/issues/4, https://github.com/mkozlovskyi-ops/course-staff-management-spec/issues/5, https://github.com/mkozlovskyi-ops/course-staff-management-spec/issues/6, https://github.com/mkozlovskyi-ops/course-staff-management-spec/issues/7, https://github.com/mkozlovskyi-ops/course-staff-management-spec/issues/8, https://github.com/mkozlovskyi-ops/course-staff-management-spec/issues/9, https://github.com/mkozlovskyi-ops/course-staff-management-spec/issues/10, https://github.com/mkozlovskyi-ops/course-staff-management-spec/issues/11, https://github.com/mkozlovskyi-ops/course-staff-management-spec/issues/12
- Зміни вимог із журналом змін: https://github.com/mkozlovskyi-ops/course-staff-management-spec/commit/5e95269a5c062f0f3d95912ec5c576f66d9cca14, https://github.com/mkozlovskyi-ops/course-staff-management-spec/commit/a7e59405cfd8f2a60a0de7e43691eee15acf89c4, https://github.com/mkozlovskyi-ops/course-staff-management-spec/commit/ea4bb85c4d26d049217bce4a36669f6fd7682860, https://github.com/mkozlovskyi-ops/course-staff-management-spec/commit/3ec0666b22774b4ab34032fb79380932865b06f3

### Нотатка PM — порівняння реалізацій (трек D)

1. **AC / вимога:** AC-BALANCE-06 — "Whole day = 1.00 day. Part of a day = the record's minutes ÷ all of the employee's working minutes on that date across the schedules of all their positions" (Assumptions in the 2026-10-19 version of https://github.com/mkozlovskyi-ops/course-staff-management-spec/blob/031e169/docs/requirements/03-leave-balance.md)
   **Як прочитали чи реалізували:** @SvitlanaHaiduchyk read the two rules as contradicting each other for a whole day of only the additional position (1.00 vs 0.15) and asked https://github.com/mkozlovskyi-ops/course-staff-management-spec/issues/7, logging "whole day is a special case of the formula" as her working assumption (https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/41990c69c8886dcb9690d029dde6c7f6f71eefcd/docs/spec-reading.md#L51). @rhutsul did not see a conflict: he applied the formula per date and capped it at 1.00 (https://github.com/rhutsul/leave-portal/blob/2b7da2f691c9d9bd0dcfb4c39bad97777d203297/docs/spec-reading.md#L65, code https://github.com/rhutsul/leave-portal/blob/2b7da2f691c9d9bd0dcfb4c39bad97777d203297/src/lib/leave/balance.ts#L42-L48).
   **Чому:** two rules for one quantity in Assumptions, with "whole day" undefined for an employee with several positions — category 9 "Термінологія" / 3 "Домен і модель даних".
   **Що зробив PM:** accepted option A in #7 and reworded the assumption (formula is the rule, 1.00 only when all of the employee's minutes are covered); no AC changed — https://github.com/mkozlovskyi-ops/course-staff-management-spec/commit/5e95269a5c062f0f3d95912ec5c576f66d9cca14
2. **AC / вимога:** AC-DECIDE-16 — "When the employee changes its hours to 09:00–12:00. Then the record has hours 09:00–12:00, keeps status "Awaiting decision"" together with the 01 assumption "Records of the same position may not overlap in time when they have status "Awaiting decision", "Returned for revision" or "Approved"" (https://github.com/mkozlovskyi-ops/course-staff-management-spec/blob/031e169/docs/requirements/02-leave-decision.md)
   **Як прочитали чи реалізували:** @rhutsul read the gap as "which fields can be edited" and asked https://github.com/mkozlovskyi-ops/course-staff-management-spec/issues/2 (recommended: hours only). @SvitlanaHaiduchyk read the same AC as unimplementable: a literal overlap check finds the edited record itself and refuses the change; she proposed A1 in a comment (https://github.com/mkozlovskyi-ops/course-staff-management-spec/issues/2) and implemented the self-exclusion (https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/41990c69c8886dcb9690d029dde6c7f6f71eefcd/src/lib/leave/overlap.ts#L49-L58). After the answer, AC-DECIDE-22 is "реалізовано" for @SvitlanaHaiduchyk and "відкладено" for @rhutsul and @lukian-tomochko (https://github.com/rhutsul/leave-portal/blob/2b7da2f691c9d9bd0dcfb4c39bad97777d203297/docs/conformance.md#L80, https://github.com/lukian-tomochko/leave-portal/blob/c0b4a397449e8209524c479d75ad02226925f793/docs/conformance.md#L78).
   **Чому:** the overlap rule did not say whether a record is checked against itself, and the matrix allowed "change own record" without naming the fields — category 7 "Граничні випадки й збої" and 1 "Функціональний обсяг".
   **Що зробив PM:** answered A + A1 in #2: only hours can be changed, the edited record is not checked against itself; added AC-DECIDE-21, AC-DECIDE-22 and assumptions in 01 and 02 — https://github.com/mkozlovskyi-ops/course-staff-management-spec/commit/5e95269a5c062f0f3d95912ec5c576f66d9cca14
3. **AC / вимога:** AC-BALANCE-11 — "Given an employee who has 5.00 days of Sick leave left on 2027-08-31 … Then "Sick leave" in school year 2027–2028 — granted 18.00, carried over 5.00, remaining 23.00", with 01 allowing past dates without limit (https://github.com/mkozlovskyi-ops/course-staff-management-spec/blob/5e95269/docs/requirements/03-leave-balance.md)
   **Як прочитали чи реалізували:** @SvitlanaHaiduchyk silently assumed that a record counts in the school year of its date, not of its approval (https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/41990c69c8886dcb9690d029dde6c7f6f71eefcd/docs/spec-reading.md#L52). @lukian-tomochko treated the same gap as open and asked https://github.com/mkozlovskyi-ops/course-staff-management-spec/issues/11 with three options (date year / approval year / forbid past years) (https://github.com/lukian-tomochko/leave-portal/blob/c0b4a397449e8209524c479d75ad02226925f793/docs/spec-reading.md#L48).
   **Чому:** the carry-over AC did not say which year a late-approved record belongs to, and nothing covered a request over 31.08/01.09 — category 7 "Граничні випадки й збої" / 5 "Нефункціональні вимоги" (time).
   **Що зробив PM:** answered A in #11 (the year of the record's date, also when approved later; a two-year request gives a record in each year); added AC-BALANCE-21, AC-BALANCE-22 and an assumption — https://github.com/mkozlovskyi-ops/course-staff-management-spec/commit/ea4bb85c4d26d049217bce4a36669f6fd7682860
4. **AC / вимога:** AC-LEAVE-06 — "When the employee tries to choose Saturday 2026-11-07. Then the date is not accepted" together with AC-LEAVE-05 (several dates in one request) (https://github.com/mkozlovskyi-ops/course-staff-management-spec/blob/5e95269/docs/requirements/01-leave-request.md)
   **Як прочитали чи реалізували:** @lukian-tomochko asked https://github.com/mkozlovskyi-ops/course-staff-management-spec/issues/9 (drop the date / block the request / skip silently) and implemented "drop the date, keep the others" (https://github.com/lukian-tomochko/leave-portal/blob/c0b4a397449e8209524c479d75ad02226925f793/docs/conformance.md#L56); @SvitlanaHaiduchyk implemented the same in her own way (https://github.com/SvitlanaHaiduchyk/course-staff-management/blob/41990c69c8886dcb9690d029dde6c7f6f71eefcd/docs/conformance.md#L98); @rhutsul has AC-LEAVE-21 "відкладено" (https://github.com/rhutsul/leave-portal/blob/2b7da2f691c9d9bd0dcfb4c39bad97777d203297/docs/conformance.md#L54).
   **Чому:** AC-LEAVE-06 described one date, AC-LEAVE-05 several, and nothing said what happens when they meet — category 4 "Потік взаємодії (UX)".
   **Що зробив PM:** answered A in #9; added AC-LEAVE-21 and the assumption "a date without working hours is dropped, the other dates stay" — https://github.com/mkozlovskyi-ops/course-staff-management-spec/commit/ea4bb85c4d26d049217bce4a36669f6fd7682860

## Розробник

—

## Бонус E

—
