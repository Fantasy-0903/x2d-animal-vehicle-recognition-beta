# 哈苏 X2D 100C 动物与车辆识别 · 测试版
**Animal & Vehicle Subject Detection for the Hasselblad X2D 100C · Test Build**

哈苏 X2D 100C 动物与车辆识别项目使用原厂相同网格规格模型，在一代机身（固件 4.2.0）上研究猫狗与车辆的机内识别。相机通过 VCR 预处理取景画面，使用一代机身的 NNF 推理框架和 DSP/CNN 加速路径运行检测，再将候选框交给机内跟踪器，在取景画面中显示选中的主体。开发时使用同一份车辆模型输出，按网格逐项核对候选框坐标，验证解码结果。

This project uses models with the same detection-grid specification as the stock system to study in-camera cat, dog and vehicle detection on the first-generation Hasselblad X2D 100C (firmware 4.2.0). The camera preprocesses live-view frames with VCR, runs detection through its NNF inference framework and DSP/CNN acceleration path, and passes candidate boxes to the tracker for display in live view. To verify decoding, we compared candidate-box coordinates at matching grid locations using the same detector output.

## 在相机上怎么用 · Camera controls

对焦点菜单引入二代式主体选择：**人物／动物（猫狗）／车辆／关**。前、后拨轮每转一格移动一项，按下后拨轮确认；回执到达时保留正在浏览的位置。菜单内半按快门返回取景。AFT 主框的位置平滑窗口从 10 次缩为 4 次；合焦后显示完整绿框，持续追焦时保持完整。

The focus-point menu offers four subjects: **Person / Animal (cats and dogs) / Vehicle / Off**. Turn either dial to move one item and click the rear dial to confirm. A model-switch acknowledgement leaves the browsing cursor in place. Half-press the shutter to return to live view. The AFT box averages four position updates instead of ten. Once focus is confirmed, the full green box stays visible during continuous tracking.

## 研究与更新进展 · Research and development

最早在取景屏上看到车辆框时，车已经移到前面，框还留在后面。R8 连续观察 90 秒，记录到 99 份结果入队。为找出等待发生在哪里，我们把模型执行、候选框解码和跟踪显示分开验证。R13 先在机内跑通完整车辆计算图，R14 再把结果接回跟踪器和取景屏；到 R15，菜单可以在猫狗和车辆检测之间切换，只运行当前选中的检测器。

In the early live-view tests, the vehicle moved ahead while its box lagged behind. R8 queued 99 results in a 90-second run. We tested inference, box decoding and tracking separately. R13 ran the complete vehicle graph on the camera; R14 fed its results back into tracking and live view. R15 added cat/dog selection and ran only the selected detector.

后续更新来自实机使用中遇到的三个问题。连续切换曾让菜单变灰，R22h 修复了关闭识别后重复销毁跟踪实例的问题。移动目标停下后框才追上，促使我们比较 2、4、10 次位置更新的平滑效果，最终在 fix2 上机确认了 4 次窗口。最近的 fix3 解决拨轮跨项：从“动物”转一格能停在“车辆”，即使上一次切换的回执刚好到达，光标也留在正在浏览的位置。

Three issues found on the camera shaped later updates. R22h fixed a repeated tracker teardown that could disable the menu after rapid switches. Lag behind moving subjects led us to compare smoothing over 2, 4 and 10 position updates; fix2 verified the four-update window on the camera. Fix3 corrected a skipped dial step: one click from Animal lands on Vehicle, and a late acknowledgement leaves the cursor there.

## 当前验证范围 · Test status

**r22l-fix3 是独立功能测试版本。** X2D 100C 实机已确认拨轮选取目标正常，并已完成撤回。4 次位置窗口和合焦完整绿框的实机验证来自 fix2，fix3 沿用了同一组件。fix3 另有 69 组离线检查。与主安装器最新版本的组合安装、户外猫狗识别质量、追踪框端到端延迟仍待验证。

**r22l-fix3 is a standalone feature test.** Dial selection and full rollback were verified on an X2D 100C. The four-update window was read back on the camera in fix2, and the operator confirmed the full green box during tracking. Fix3 retains those components and passed 69 offline check groups. Integration with the main installer, outdoor cat/dog recognition quality and end-to-end tracking latency remain open.

## 测试包获取 · Test build access

本项目目前为独立功能测试。测试包由项目维护者单独分享给参与测试的人。如需参与，可联系维护者申请访问。

This is a standalone feature test. The maintainer shares the test build directly with participants. Contact the maintainer to request access.
