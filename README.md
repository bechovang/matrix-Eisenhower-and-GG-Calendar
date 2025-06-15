# Ứng Dụng Quản Lý Task & Lịch (Ma trận Eisenhower + Calendar Clone)

## Mục lục

1. [Tổng quan & Mục tiêu](#tổng-quan--mục-tiêu)
2. [Module chính](#module-chính)
   * Ma trận Eisenhower
   * Calendar "clone" Google Calendar
   * Hệ thống Tab phân loại
3. [Chi tiết UI/UX Ma trận Eisenhower](#chi-tiết-uiux-ma-trận-eisenhower)
4. [Chi tiết UI/UX Calendar Clone](#chi-tiết-uiux-calendar-clone)
5. [Hệ thống Tab & Phân loại](#hệ-thống-tab--phân-loại)
6. [Công nghệ & Thành phần](#công-nghệ--thành-phần)
7. [Cấu trúc Component](#cấu-trúc-component)
8. [Luồng tương tác (UX Flow)](#luồng-tương-tác-ux-flow)
9. [Timeline & Sprint Plan](#timeline--sprint-plan)

---

## 1. Tổng quan & Mục tiêu

Ứng dụng hướng đến:

* **Ma trận Eisenhower**: Nơi người dùng tạo, ghi chú, planning các task chưa có lịch.
* **Calendar Clone**: Giao diện lịch Month/Week/Day, nhập thủ công event với form.
* **Hệ thống Tab**: Phân loại task theo: Tất cả, Bản thân, Gia đình, Công việc, v.v.

**Yêu cầu**:

* Tạo/Sửa/Xóa task (title, mô tả, tag, urgency/importance, category).
* Tạo/Sửa/Xóa event thông qua form nhập liệu (date, time, category).
* Drag–drop chỉ trong Matrix (giữa các quadrant).
* Hệ thống filter theo category tabs.
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
* Filter theo category tabs.

### 2.2 Calendar Clone

* Hiển thị lịch Month/Week/Day view.
* **Tạo event**: Nhấn vào ngày/slot thời gian → mở form nhập thông tin.
* **Chỉnh sửa event**: Click vào event → form edit.
* Event hiển thị màu sắc theo category.
* **Không có drag-drop**: Tất cả thao tác thông qua form input.

### 2.3 Hệ thống Tab phân loại

* **Tất cả**: Hiển thị toàn bộ task/event.
* **Bản thân**: Task/event cá nhân, phát triển bản thân.
* **Gia đình**: Công việc gia đình, họp mặt, sinh nhật.
* **Công việc**: Meeting, deadline, dự án.
* **Sức khỏe**: Gym, khám bệnh, thuốc.
* **Học tập**: Khóa học, đọc sách, kỹ năng.

---

## 3. Chi tiết UI/UX Ma trận Eisenhower

1. **Trực quan & Rõ ràng**
   * 4 ô grid 2×2; mỗi ô màu nền dịu, viền rõ.
   * Header ô: "Q1 – Gấp & Quan trọng", v.v.
   * Tab bar phía trên để filter theo category.

2. **Tương tác mượt mà**
   * Thư viện: `react-beautiful-dnd` hoặc `@dnd-kit/core`.
   * Animation với Framer Motion: lift/drop card, ghost preview.
   * Chỉ drag-drop trong Matrix (không với Calendar).

3. **Task Card**
   * Hiển thị: tiêu đề, deadline (nếu có), category badge, tag icon.
   * Kéo-thả: z-index cao, phóng to 5–10% khi active.
   * Color coding theo category.

4. **Feedback**
   * Tooltip "Chuyển sang Q2" khi drop.
   * Toast "Đã lưu" sau update.

5. **Responsive & Accessibility**
   * Mobile: 4 sections dọc, drag–drop touch, swipe.
   * Keyboard: Tab + Space/Enter pick & drop.
   * ARIA labels cho quadrant & card.

---

## 4. Chi tiết UI/UX Calendar Clone

1. **Calendar View**
   * Month/Week/Day views với navigation.
   * Clean UI tương tự Google Calendar.
   * Event hiển thị với màu sắc theo category.

2. **Tạo Event**
   * Click vào ngày/slot → mở modal form.
   * Form fields: Title, Description, Start Date/Time, End Date/Time, Category, Priority.
   * Validation: required fields, time logic.

3. **Chỉnh sửa Event**
   * Click vào event → modal edit với thông tin sẵn có.
   * Buttons: Save, Delete, Cancel.

4. **Event Display**
   * Hiển thị title + time.
   * Hover → tooltip với full details.
   * Color coding theo category.

5. **Responsive**
   * Mobile: Week/Day view ưu tiên, month view thu gọn.
   * Form modal responsive với touch-friendly inputs.

---

## 5. Hệ thống Tab & Phân loại

1. **Tab Navigation**
   * Horizontal tabs: Tất cả | Bản thân | Gia đình | Công việc | Sức khỏe | Học tập
   * Active tab highlight với underline animation.
   * Badge count cho mỗi category.

2. **Color System**
   * Tất cả: Gray
   * Bản thân: Blue
   * Gia đình: Green  
   * Công việc: Purple
   * Sức khỏe: Red
   * Học tập: Orange

3. **Filter Logic**
   * Matrix: Chỉ hiển thị task thuộc category đang chọn.
   * Calendar: Chỉ hiển thị event thuộc category đang chọn.
   * Form: Dropdown chọn category khi tạo mới.

---

## 6. Công nghệ & Thành phần

| Thành phần       | Giải pháp gợi ý                         |
| ---------------- | --------------------------------------- |
| Drag & Drop      | react-beautiful-dnd / @dnd-kit/core (chỉ Matrix) |
| Animation        | framer-motion                           |
| Styling          | Tailwind CSS + shadcn/ui + lucide-react |
| Calendar UI      | React Big Calendar / tự build          |
| State Management | Zustand hoặc Redux Toolkit              |
| Icons            | lucide-react                            |
| Form Validation  | react-hook-form + zod                   |

---

## 7. Cấu trúc Component

```jsx
<App>
  <Header />
  <TabNavigation 
    tabs={['all', 'personal', 'family', 'work', 'health', 'learning']}
    activeTab={activeCategory}
    onTabChange={setActiveCategory}
  />
  
  <ViewToggle tabs={['Matrix', 'Calendar']} />
  
  {mode === 'Matrix' && (
    <MatrixView 
      tasks={filteredTasks} 
      category={activeCategory}
      onTaskMove={handleTaskMove}
    />
  )}
  
  {mode === 'Calendar' && (
    <CalendarView
      events={filteredEvents}
      category={activeCategory}
      onEventCreate={handleEventCreate}
      onEventEdit={handleEventEdit}
    />
  )}
  
  <TaskModal />
  <EventModal />
</App>
```

* **MatrixView**: Grid 2×2, each quadrant (`<MatrixQuadrant>`) chứa `<TaskCardList>`.
* **CalendarView**: `<CalendarHeader />` + `<CalendarGrid />` + form modals.
* **TabNavigation**: Category filter tabs với badges.

---

## 8. Luồng tương tác (UX Flow)

1. **Chọn Category**:
   * Click tab → filter tasks/events theo category.

2. **Tạo task** (Matrix):
   * Nhấn "+ Task" → form modal với category dropdown → tạo task.

3. **Phân loại** (Matrix):
   * Kéo-thả giữa Q1–Q4 → update urgency/importance.

4. **Tạo Event** (Calendar):
   * Click vào ngày/slot → form modal → nhập thông tin → Save.

5. **Chỉnh sửa Event**:
   * Click event → edit modal → update thông tin → Save/Delete.

6. **Chuyển đổi View**:
   * Toggle Matrix ↔ Calendar giữ nguyên category filter.

---
