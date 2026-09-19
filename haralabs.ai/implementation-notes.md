run haralytics.py
confirm 'pest infestation' sample drone images exist in /Users/kchekuru/src/ultralytics/ultralytics/assets/
confirm sample image exists '/Users/kchekuru/src/ultralytics/ultralytics/assets/bus.jpg'
this creates haralytics model '/Users/kchekuru/src/ultralytics/haralabs.ai/yolov8n.onnx' using pre-trained open ultralytics model '/Users/kchekuru/src/ultralytics/haralabs.ai/yolov8n.pt'
-- inference --
/Users/kchekuru/src/ultralytics/.venv/bin/python -c "
import onnxruntime as ort
sess = ort.InferenceSession('/Users/kchekuru/src/ultralytics/haralabs.ai/yolov8n.onnx')
for i in sess.get_inputs():
    print(i.name, i.shape, i.type)
"
cd /Users/kchekuru/src/ultralytics/examples/YOLOv8-ONNXRuntime-Rust
source "$HOME/.cargo/env"
export ORT_DYLIB_PATH=/Users/kchekuru/src/ultralytics/.venv/lib/python3.13/site-packages/onnxruntime/capi/libonnxruntime.1.30.0.dylib

cargo run --release -- \
  --model /Users/kchekuru/src/ultralytics/haralabs.ai/yolov8n.onnx \
  --source /Users/kchekuru/src/ultralytics/ultralytics/assets/bus.jpg \
  --height 640 --width 640

[Notice above haralabs.ai trained model is leveraged]




--- future enhancements --
(.venv) kchekuru@s-MacBook-Air haralytics % ls haralytics 
__init__.py     cfg             models          py.typed        utils
__pycache__     data            nn              solutions
assets          engine          optim           trackers

cargo run --release -- \
  --model /Users/kchekuru/src/haralytics/yolov8n.onnx \
  --source /Users/kchekuru/src/haralytics/haralytics/assets/bus.jpg
  cargo run --release -- \
  --model /Users/kchekuru/src/haralytics/yolov8n.onnx \
  --source /Users/kchekuru/src/haralytics/haralytics/assets/psrnlr-farm-8-daily-pest-check-feet.jpg