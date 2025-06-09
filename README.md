thêm phần tab : tất cả , bản thân, gia đình,...

## Ứng Dụng Quản Lý Task & Lịch (Ma trận Eisenhower + Calendar Clone)

### Mục lục

1. [Tổng quan & Mục tiêu](#tổng-quan--mục-tiêu)
2. [Module chính](#module-chính)

   * Ma trận Eisenhower
   * Calendar “clone” Google Calendar
3. [Chi tiết UI/UX Ma trận Eisenhower](#chi-tiết-uiux-ma-trận-eisenhower)
4. [Chi tiết UI/UX Calendar Clone](#chi-tiết-uiux-calendar-clone)
5. [Công nghệ & Thành phần](#công-nghệ--thành-phần)
6. [Cấu trúc Component](#cấu-trúc-component)
7. [Luồng tương tác (UX Flow)](#luồng-tương-tác-ux-flow)
8. [Timeline & Sprint Plan](#timeline--sprint-plan)

---

## 1. Tổng quan & Mục tiêu

Ứng dụng hướng đến:

* **Ma trận Eisenhower**: Nơi người dùng tạo, ghi chú, planning các task chưa có lịch.
* **Calendar Clone**: Giao diện lịch Month/Week/Day, kéo-thả task từ Matrix để lên lịch.

**Yêu cầu**:

* Tạo/Sửa/Xóa task (title, mô tả, tag, urgency/importance).
* Tạo/Sửa/Xóa event (date, time).
* Drag–drop giữa Matrix ↔ Calendar.
* Responsive & Accessibility (mobile, keyboard, ARIA).

---

## 2. Module chính

### 2.1 Ma trận Eisenhower

* 4 quadrants theo 2 trục **Gấp** (urgency) & **Quan trọng** (importance):

  * Q1: Gấp & Quan trọng
  * Q2: Không gấp & Quan trọng
  * Q3: Gấp & Không quan trọng
  * Q4: Không gấp & Không quan trọng
* Người dùng tự kéo-thả task giữa 4 ô để phân loại.
* Task chưa scheduling: hiển thị badge "Unscheduled".

### 2.2 Calendar Clone

* Hiển thị lịch Month/Week/Day (FullCalendar.js hoặc React Big Calendar).
* Sidebar: danh sách task chưa scheduling.
* Drag–drop task từ sidebar vào slot giờ để lên lịch.
* Event đã lập lịch: hiển thị màu sắc theo priority.

---

## 3. Chi tiết UI/UX Ma trận Eisenhower

1. **Trực quan & Rõ ràng**

   * 4 ô grid 2×2; mỗi ô màu nền dịu, viền rõ.
   * Header ô: "Q1 – Gấp & Quan trọng", v.v.

2. **Tương tác mượt mà**

   * Thư viện: `react-beautiful-dnd` hoặc `@dnd-kit/core`.
   * Animation với Framer Motion: lift/drop card, ghost preview.

3. **Task Card**

   * Hiển thị: tiêu đề, deadline (nếu có), tag icon.
   * Kéo-thả: z-index cao, phóng to 5–10% khi active.

4. **Feedback**

   * Tooltip "Chuyển sang Q2" khi drop.
   * Toast "Đã lưu" sau update.

5. **Responsive & Accessibility**

   * Mobile: 4 sections dọc, drag–drop touch, swipe.
   * Keyboard: Tab + Space/Enter pick & drop.
   * ARIA labels cho quadrant & card.

---

## 4. Chi tiết UI/UX Calendar Clone

1. **Sidebar Task List**

   * Danh sách task chưa scheduled, draggable.
   * Badge "Unscheduled".

2. **Calendar View**

   * FullCalendar.js: Month/Week/Day views.
   * Time slots droppable.

3. **Drag & Drop Flow**

   * Kéo task từ sidebar → slot ngày/giờ.
   * Highlight ô thả.
   * Thả → modal confirm thời gian, nhấn "Confirm" → lên lịch.

4. **Event Interactions**

   * Kéo event đổi slot.
   * Click event để edit hoặc unschedule.

5. **Responsive**

   * Mobile: Week List view, sidebar collapsible drawer.

---

## 5. Công nghệ & Thành phần

| Thành phần       | Giải pháp gợi ý                         |
| ---------------- | --------------------------------------- |
| Drag & Drop      | react-beautiful-dnd / @dnd-kit/core     |
| Animation        | framer-motion                           |
| Styling          | Tailwind CSS + shadcn/ui + lucide-react |
| Calendar UI      | FullCalendar.js / React Big Calendar    |
| State Management | Zustand hoặc Redux Toolkit              |
| Icons            | lucide-react                            |

---

## 6. Cấu trúc Component

```jsx
<App>
  <Header tabs={[ 'Matrix', 'Calendar' ]} />
  {mode === 'Matrix' && <MatrixView tasks={tasks} />}
  {mode === 'Calendar' && (
    <CalendarView
      unscheduledTasks={tasks.filter(t => !t.scheduled)}
      events={tasks.filter(t => t.scheduled)}
      onTaskDrop={handleSchedule}
    />
  )}
</App>
```

* **MatrixView**: Grid 2×2, each quadrant (`<MatrixQuadrant>`) chứa `<TaskCardList>`.
* **CalendarView**: `<SidebarTaskList />` + `<FullCalendar />`.

---

## 7. Luồng tương tác (UX Flow)

1. **Tạo task** (Matrix):

   * Nhấn "+ Task" → form modal → tạo task với title/mô tả/tag.

2. **Phân loại tạm** (Matrix):

   * Kéo-thả giữa Q1–Q4 → update urgency/importance.

3. **Lên lịch** (Calendar):

   * Chuyển sang Calendar tab → kéo từ sidebar vào slot.
   * Modal confirm thời gian → nhấn Confirm.

4. **Chỉnh sửa lịch**:

   * Kéo event đổi slot → update start/end.
   * Click event → edit form hoặc unschedule.

5. **Xem thông tin**:

   * Quay lại Matrix → card có badge "Scheduled".
   * Quay Calendar → chỉ hiển thị events.

---

## 8. Timeline & Sprint Plan

| Tuần | Công việc chính                                                         |
| :--: | :---------------------------------------------------------------------- |
|   1  | Wireframe & prototype Figma (Matrix + Calendar)                         |
|   2  | Triển khai MatrixView: grid + drag–drop + animations                    |
|   3  | Triển khai CalendarView: sidebar + FullCalendar + drag–drop integration |
|   4  | Modal confirm, edit/unschedule, toast, tooltip                          |
|   5  | Responsive & accessibility (mobile view, keyboard support, ARIA)        |
|   6  | Kiểm thử, bugfix, polish UI, deploy frontend (Vercel)                   |

---

**Chú ý:**

* Sprint Planning & Review hàng tuần.
* Daily stand-up ngắn (10 phút).
* Liên tục test UX với người dùng mẫu.
