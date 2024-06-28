11111
#include <iostream>
#include <string>
#include <android/log.h>

#define TAG_LOGI(tag, fmt, ...) __android_log_print(ANDROID_LOG_INFO, tag, "xql: " fmt, ##__VA_ARGS__)
#define TAG_LOGW(tag, fmt, ...) __android_log_print(ANDROID_LOG_WARN, tag, "xql: " fmt, ##__VA_ARGS__)

enum class AceLogTag {
    ACE_KEYBOARD,
    ACE_FOCUS,
    // 其他标签...
};

// 假设有一个函数将 AceLogTag 转换为字符串
const char* GetTagString(AceLogTag tag) {
    switch (tag) {
        case AceLogTag::ACE_KEYBOARD: return "ACE_KEYBOARD";
        case AceLogTag::ACE_FOCUS: return "ACE_FOCUS";
        // 其他标签...
        default: return "UNKNOWN_TAG";
    }
}

// 假设 OffsetF 和 RectF 类已经定义
class OffsetF {
public:
    OffsetF(float x = 0, float y = 0) : x_(x), y_(y) {}
    float GetX() const { return x_; }
    float GetY() const { return y_; }
    std::string ToString() const { return "(" + std::to_string(x_) + ", " + std::to_string(y_) + ")"; }
private:
    float x_;
    float y_;
};

class RectF {
public:
    RectF(OffsetF offset = OffsetF(), float width = 0, float height = 0)
        : offset_(offset), width_(width), height_(height) {}
    OffsetF Center() const { return OffsetF(offset_.GetX() + width_ / 2, offset_.GetY() + height_ / 2); }
    OffsetF GetOffset() const { return offset_; }
private:
    OffsetF offset_;
    float width_;
    float height_;
};

int main() {
    // 示例变量
    OffsetF childRectCenter(10, 20);
    OffsetF rectCenter(5, 15);
    RectF childRect(childRectCenter, 100, 200);
    RectF rect(rectCenter, 50, 100);
    OffsetF vec = childRect.Center() - rect.Center();
    double minVal = 100.0;
    double val = (vec.GetX() * vec.GetX()) + (vec.GetY() * vec.GetY());
    auto it = 42; // 示例迭代器
    auto itNewFocusNode = it;
    OffsetF offset;

    // 记录 vec 的值
    TAG_LOGI(GetTagString(AceLogTag::ACE_FOCUS), "vec: (%{public}s).", vec.ToString().c_str());

    // 记录 val 的值
    TAG_LOGI(GetTagString(AceLogTag::ACE_FOCUS), "val: %{public}f.", val);

    // 记录 minVal 的值
    TAG_LOGI(GetTagString(AceLogTag::ACE_FOCUS), "minVal: %{public}f.", minVal);

    // 记录 itNewFocusNode 的值
    TAG_LOGI(GetTagString(AceLogTag::ACE_FOCUS), "itNewFocusNode: %{public}d.", itNewFocusNode);

    // 记录 offset 的值
    TAG_LOGI(GetTagString(AceLogTag::ACE_FOCUS), "offset: (%{public}s).", offset.ToString().c_str());

    // 示例代码逻辑
    if (minVal > val) {
        minVal = val;
        itNewFocusNode = it;
        offset = childRect.GetOffset();
    }

    return 0;
}
