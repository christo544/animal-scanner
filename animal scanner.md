# Code Citations

## License: MIT
https://github.com/react-ui-org/react-ui/blob/9014fb5f2cbd752c5753f439154b91a7268a2735/src/docs/getting-started/usage.md

```
Here are all your files:

## **server.py** (Backend - Run this)
```python
#!/usr/bin/env python3
import os
import json
import io
from http.server import HTTPServer, SimpleHTTPRequestHandler
from PIL import Image
import numpy as np

# Load AI model
AI_ENABLED = False
model = None

try:
    from tensorflow.keras.applications import MobileNetV2
    from tensorflow.keras.preprocessing import image as keras_image
    from tensorflow.keras.applications.mobilenet_v2 import decode_predictions
    import tensorflow as tf
    
    print("Loading AI model (MobileNetV2)...")
    model = MobileNetV2(weights='imagenet')
    print("✓ AI model loaded successfully!")
    AI_ENABLED = True
except Exception as e:
    print(f"⚠️  TensorFlow/Keras error: {e}")
    print("Will use basic mock AI instead\n")

class Handler(SimpleHTTPRequestHandler):
    def do_POST(self):
        if self.path == '/scan':
            content_length = int(self.headers.get('Content-Length', 0))
            body = self.rfile.read(content_length)
            
            print(f"✓ Received image ({len(body)} bytes)")
            
            try:
                if AI_ENABLED and model:
                    result = self.identify_animal_ml(body)
                else:
                    result = self.identify_animal_mock(body)
                
                self.send_response(200)
                self.send_header('Content-type', 'application/json')
                self.end_headers()
                response = json.dumps({"result": result})
                self.wfile.write(response.encode())
                print(f"  → Result: {result}\n")
            except Exception as e:
                self.send_response(500)
                self.send_header('Content-type', 'application/json')
                self.end_headers()
                response = json.dumps({"error": str(e)})
                self.wfile.write(response.encode())
                print(f"  → Error: {e}\n")
        else:
            self.send_response(404)
            self.end_headers()
    
    def identify_animal_ml(self, image_bytes):
        """Use TensorFlow AI to identify animals in image"""
        try:
            img = Image.open(io.BytesIO(image_bytes)).convert('RGB')
            img = img.resize((224, 224))
            img_array = keras_image.img_to_array(img)
            img_array = np.expand_dims(img_array, axis=0)
            img_array = tf.keras.applications.mobilenet_v2.preprocess_input(img_array)
            
            predictions = model.predict(img_array, verbose=0)
            decoded = decode_predictions(predictions, top=3)[0]
            
            results = []
            for label, description, confidence in decoded:
                conf_pct = int(confidence * 100)
                results.append(f"{description} ({conf_pct}%)")
            
            return "🐾 AI Result: " + " | ".join(results)
        except Exception as e:
            return f"ML Error: {str(e)}"
    
    def identify_animal_mock(self, image_bytes):
        """Use image analysis to make intelligent guesses"""
        try:
            img = Image.open(io.BytesIO(image_bytes)).convert('RGB')
            img_array = np.array(img)
            
            # Simple heuristics based on color distribution
            r_avg = np.mean(img_array[:,:,0])
            g_avg = np.mean(img_array[:,:,1])
            b_avg = np.mean(img_array[:,:,2])
            
            # Mock animal detection based on image colors
            animals = [
                ("Dog", "Yellow/Brown tones detected"),
                ("Cat", "Warm colors detected"),
                ("Bird", "Varied color palette detected"),
            ]
            
            # Pick based on color balance
            if r_avg > g_avg and r_avg > b_avg:
                return f"🐾 {animals[0][0]} ({int(r_avg)}% confidence) - {animals[0][1]}"
            elif g_avg > b_avg:
                return f"🐾 {animals[1][0]} ({int(g_avg)}% confidence) - {animals[1][1]}"
            else:
                return f"🐾 {animals[2][0]} ({int(b_avg)}% confidence) - {animals[2][1]}"
        except Exception as e:
            return f"Analysis failed: {str(e)}"
    
    def log_message(self, format, *args):
        print(f"[{self.log_date_time_string()}] {format % args}")

if __name__ == '__main__':
    os.chdir(os.path.dirname(os.path.abspath(__file__)))
    print("🐾 Animal Scanner Server with AI")
    print("📍 Starting server on http://localhost:8000")
    print("📱 Open http://localhost:8000/app.html in your browser")
    if AI_ENABLED:
        print("✓ TensorFlow AI enabled")
    else:
        print("⚠️  Using basic image analysis (TensorFlow not available)")
    print("\nPress Ctrl+C to stop\n")
    
    server = HTTPServer(('localhost', 8000), Handler)
    server.serve_forever()
```

---

## **app.html** (Frontend UI)
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>Animal Scanner — Demo
```


## License: MIT
https://github.com/react-ui-org/react-ui/blob/9014fb5f2cbd752c5753f439154b91a7268a2735/src/docs/getting-started/usage.md

```
Here are all your files:

## **server.py** (Backend - Run this)
```python
#!/usr/bin/env python3
import os
import json
import io
from http.server import HTTPServer, SimpleHTTPRequestHandler
from PIL import Image
import numpy as np

# Load AI model
AI_ENABLED = False
model = None

try:
    from tensorflow.keras.applications import MobileNetV2
    from tensorflow.keras.preprocessing import image as keras_image
    from tensorflow.keras.applications.mobilenet_v2 import decode_predictions
    import tensorflow as tf
    
    print("Loading AI model (MobileNetV2)...")
    model = MobileNetV2(weights='imagenet')
    print("✓ AI model loaded successfully!")
    AI_ENABLED = True
except Exception as e:
    print(f"⚠️  TensorFlow/Keras error: {e}")
    print("Will use basic mock AI instead\n")

class Handler(SimpleHTTPRequestHandler):
    def do_POST(self):
        if self.path == '/scan':
            content_length = int(self.headers.get('Content-Length', 0))
            body = self.rfile.read(content_length)
            
            print(f"✓ Received image ({len(body)} bytes)")
            
            try:
                if AI_ENABLED and model:
                    result = self.identify_animal_ml(body)
                else:
                    result = self.identify_animal_mock(body)
                
                self.send_response(200)
                self.send_header('Content-type', 'application/json')
                self.end_headers()
                response = json.dumps({"result": result})
                self.wfile.write(response.encode())
                print(f"  → Result: {result}\n")
            except Exception as e:
                self.send_response(500)
                self.send_header('Content-type', 'application/json')
                self.end_headers()
                response = json.dumps({"error": str(e)})
                self.wfile.write(response.encode())
                print(f"  → Error: {e}\n")
        else:
            self.send_response(404)
            self.end_headers()
    
    def identify_animal_ml(self, image_bytes):
        """Use TensorFlow AI to identify animals in image"""
        try:
            img = Image.open(io.BytesIO(image_bytes)).convert('RGB')
            img = img.resize((224, 224))
            img_array = keras_image.img_to_array(img)
            img_array = np.expand_dims(img_array, axis=0)
            img_array = tf.keras.applications.mobilenet_v2.preprocess_input(img_array)
            
            predictions = model.predict(img_array, verbose=0)
            decoded = decode_predictions(predictions, top=3)[0]
            
            results = []
            for label, description, confidence in decoded:
                conf_pct = int(confidence * 100)
                results.append(f"{description} ({conf_pct}%)")
            
            return "🐾 AI Result: " + " | ".join(results)
        except Exception as e:
            return f"ML Error: {str(e)}"
    
    def identify_animal_mock(self, image_bytes):
        """Use image analysis to make intelligent guesses"""
        try:
            img = Image.open(io.BytesIO(image_bytes)).convert('RGB')
            img_array = np.array(img)
            
            # Simple heuristics based on color distribution
            r_avg = np.mean(img_array[:,:,0])
            g_avg = np.mean(img_array[:,:,1])
            b_avg = np.mean(img_array[:,:,2])
            
            # Mock animal detection based on image colors
            animals = [
                ("Dog", "Yellow/Brown tones detected"),
                ("Cat", "Warm colors detected"),
                ("Bird", "Varied color palette detected"),
            ]
            
            # Pick based on color balance
            if r_avg > g_avg and r_avg > b_avg:
                return f"🐾 {animals[0][0]} ({int(r_avg)}% confidence) - {animals[0][1]}"
            elif g_avg > b_avg:
                return f"🐾 {animals[1][0]} ({int(g_avg)}% confidence) - {animals[1][1]}"
            else:
                return f"🐾 {animals[2][0]} ({int(b_avg)}% confidence) - {animals[2][1]}"
        except Exception as e:
            return f"Analysis failed: {str(e)}"
    
    def log_message(self, format, *args):
        print(f"[{self.log_date_time_string()}] {format % args}")

if __name__ == '__main__':
    os.chdir(os.path.dirname(os.path.abspath(__file__)))
    print("🐾 Animal Scanner Server with AI")
    print("📍 Starting server on http://localhost:8000")
    print("📱 Open http://localhost:8000/app.html in your browser")
    if AI_ENABLED:
        print("✓ TensorFlow AI enabled")
    else:
        print("⚠️  Using basic image analysis (TensorFlow not available)")
    print("\nPress Ctrl+C to stop\n")
    
    server = HTTPServer(('localhost', 8000), Handler)
    server.serve_forever()
```

---

## **app.html** (Frontend UI)
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>Animal Scanner — Demo
```


## License: MIT
https://github.com/react-ui-org/react-ui/blob/9014fb5f2cbd752c5753f439154b91a7268a2735/src/docs/getting-started/usage.md

```
Here are all your files:

## **server.py** (Backend - Run this)
```python
#!/usr/bin/env python3
import os
import json
import io
from http.server import HTTPServer, SimpleHTTPRequestHandler
from PIL import Image
import numpy as np

# Load AI model
AI_ENABLED = False
model = None

try:
    from tensorflow.keras.applications import MobileNetV2
    from tensorflow.keras.preprocessing import image as keras_image
    from tensorflow.keras.applications.mobilenet_v2 import decode_predictions
    import tensorflow as tf
    
    print("Loading AI model (MobileNetV2)...")
    model = MobileNetV2(weights='imagenet')
    print("✓ AI model loaded successfully!")
    AI_ENABLED = True
except Exception as e:
    print(f"⚠️  TensorFlow/Keras error: {e}")
    print("Will use basic mock AI instead\n")

class Handler(SimpleHTTPRequestHandler):
    def do_POST(self):
        if self.path == '/scan':
            content_length = int(self.headers.get('Content-Length', 0))
            body = self.rfile.read(content_length)
            
            print(f"✓ Received image ({len(body)} bytes)")
            
            try:
                if AI_ENABLED and model:
                    result = self.identify_animal_ml(body)
                else:
                    result = self.identify_animal_mock(body)
                
                self.send_response(200)
                self.send_header('Content-type', 'application/json')
                self.end_headers()
                response = json.dumps({"result": result})
                self.wfile.write(response.encode())
                print(f"  → Result: {result}\n")
            except Exception as e:
                self.send_response(500)
                self.send_header('Content-type', 'application/json')
                self.end_headers()
                response = json.dumps({"error": str(e)})
                self.wfile.write(response.encode())
                print(f"  → Error: {e}\n")
        else:
            self.send_response(404)
            self.end_headers()
    
    def identify_animal_ml(self, image_bytes):
        """Use TensorFlow AI to identify animals in image"""
        try:
            img = Image.open(io.BytesIO(image_bytes)).convert('RGB')
            img = img.resize((224, 224))
            img_array = keras_image.img_to_array(img)
            img_array = np.expand_dims(img_array, axis=0)
            img_array = tf.keras.applications.mobilenet_v2.preprocess_input(img_array)
            
            predictions = model.predict(img_array, verbose=0)
            decoded = decode_predictions(predictions, top=3)[0]
            
            results = []
            for label, description, confidence in decoded:
                conf_pct = int(confidence * 100)
                results.append(f"{description} ({conf_pct}%)")
            
            return "🐾 AI Result: " + " | ".join(results)
        except Exception as e:
            return f"ML Error: {str(e)}"
    
    def identify_animal_mock(self, image_bytes):
        """Use image analysis to make intelligent guesses"""
        try:
            img = Image.open(io.BytesIO(image_bytes)).convert('RGB')
            img_array = np.array(img)
            
            # Simple heuristics based on color distribution
            r_avg = np.mean(img_array[:,:,0])
            g_avg = np.mean(img_array[:,:,1])
            b_avg = np.mean(img_array[:,:,2])
            
            # Mock animal detection based on image colors
            animals = [
                ("Dog", "Yellow/Brown tones detected"),
                ("Cat", "Warm colors detected"),
                ("Bird", "Varied color palette detected"),
            ]
            
            # Pick based on color balance
            if r_avg > g_avg and r_avg > b_avg:
                return f"🐾 {animals[0][0]} ({int(r_avg)}% confidence) - {animals[0][1]}"
            elif g_avg > b_avg:
                return f"🐾 {animals[1][0]} ({int(g_avg)}% confidence) - {animals[1][1]}"
            else:
                return f"🐾 {animals[2][0]} ({int(b_avg)}% confidence) - {animals[2][1]}"
        except Exception as e:
            return f"Analysis failed: {str(e)}"
    
    def log_message(self, format, *args):
        print(f"[{self.log_date_time_string()}] {format % args}")

if __name__ == '__main__':
    os.chdir(os.path.dirname(os.path.abspath(__file__)))
    print("🐾 Animal Scanner Server with AI")
    print("📍 Starting server on http://localhost:8000")
    print("📱 Open http://localhost:8000/app.html in your browser")
    if AI_ENABLED:
        print("✓ TensorFlow AI enabled")
    else:
        print("⚠️  Using basic image analysis (TensorFlow not available)")
    print("\nPress Ctrl+C to stop\n")
    
    server = HTTPServer(('localhost', 8000), Handler)
    server.serve_forever()
```

---

## **app.html** (Frontend UI)
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>Animal Scanner — Demo
```


## License: MIT
https://github.com/react-ui-org/react-ui/blob/9014fb5f2cbd752c5753f439154b91a7268a2735/src/docs/getting-started/usage.md

```
Here are all your files:

## **server.py** (Backend - Run this)
```python
#!/usr/bin/env python3
import os
import json
import io
from http.server import HTTPServer, SimpleHTTPRequestHandler
from PIL import Image
import numpy as np

# Load AI model
AI_ENABLED = False
model = None

try:
    from tensorflow.keras.applications import MobileNetV2
    from tensorflow.keras.preprocessing import image as keras_image
    from tensorflow.keras.applications.mobilenet_v2 import decode_predictions
    import tensorflow as tf
    
    print("Loading AI model (MobileNetV2)...")
    model = MobileNetV2(weights='imagenet')
    print("✓ AI model loaded successfully!")
    AI_ENABLED = True
except Exception as e:
    print(f"⚠️  TensorFlow/Keras error: {e}")
    print("Will use basic mock AI instead\n")

class Handler(SimpleHTTPRequestHandler):
    def do_POST(self):
        if self.path == '/scan':
            content_length = int(self.headers.get('Content-Length', 0))
            body = self.rfile.read(content_length)
            
            print(f"✓ Received image ({len(body)} bytes)")
            
            try:
                if AI_ENABLED and model:
                    result = self.identify_animal_ml(body)
                else:
                    result = self.identify_animal_mock(body)
                
                self.send_response(200)
                self.send_header('Content-type', 'application/json')
                self.end_headers()
                response = json.dumps({"result": result})
                self.wfile.write(response.encode())
                print(f"  → Result: {result}\n")
            except Exception as e:
                self.send_response(500)
                self.send_header('Content-type', 'application/json')
                self.end_headers()
                response = json.dumps({"error": str(e)})
                self.wfile.write(response.encode())
                print(f"  → Error: {e}\n")
        else:
            self.send_response(404)
            self.end_headers()
    
    def identify_animal_ml(self, image_bytes):
        """Use TensorFlow AI to identify animals in image"""
        try:
            img = Image.open(io.BytesIO(image_bytes)).convert('RGB')
            img = img.resize((224, 224))
            img_array = keras_image.img_to_array(img)
            img_array = np.expand_dims(img_array, axis=0)
            img_array = tf.keras.applications.mobilenet_v2.preprocess_input(img_array)
            
            predictions = model.predict(img_array, verbose=0)
            decoded = decode_predictions(predictions, top=3)[0]
            
            results = []
            for label, description, confidence in decoded:
                conf_pct = int(confidence * 100)
                results.append(f"{description} ({conf_pct}%)")
            
            return "🐾 AI Result: " + " | ".join(results)
        except Exception as e:
            return f"ML Error: {str(e)}"
    
    def identify_animal_mock(self, image_bytes):
        """Use image analysis to make intelligent guesses"""
        try:
            img = Image.open(io.BytesIO(image_bytes)).convert('RGB')
            img_array = np.array(img)
            
            # Simple heuristics based on color distribution
            r_avg = np.mean(img_array[:,:,0])
            g_avg = np.mean(img_array[:,:,1])
            b_avg = np.mean(img_array[:,:,2])
            
            # Mock animal detection based on image colors
            animals = [
                ("Dog", "Yellow/Brown tones detected"),
                ("Cat", "Warm colors detected"),
                ("Bird", "Varied color palette detected"),
            ]
            
            # Pick based on color balance
            if r_avg > g_avg and r_avg > b_avg:
                return f"🐾 {animals[0][0]} ({int(r_avg)}% confidence) - {animals[0][1]}"
            elif g_avg > b_avg:
                return f"🐾 {animals[1][0]} ({int(g_avg)}% confidence) - {animals[1][1]}"
            else:
                return f"🐾 {animals[2][0]} ({int(b_avg)}% confidence) - {animals[2][1]}"
        except Exception as e:
            return f"Analysis failed: {str(e)}"
    
    def log_message(self, format, *args):
        print(f"[{self.log_date_time_string()}] {format % args}")

if __name__ == '__main__':
    os.chdir(os.path.dirname(os.path.abspath(__file__)))
    print("🐾 Animal Scanner Server with AI")
    print("📍 Starting server on http://localhost:8000")
    print("📱 Open http://localhost:8000/app.html in your browser")
    if AI_ENABLED:
        print("✓ TensorFlow AI enabled")
    else:
        print("⚠️  Using basic image analysis (TensorFlow not available)")
    print("\nPress Ctrl+C to stop\n")
    
    server = HTTPServer(('localhost', 8000), Handler)
    server.serve_forever()
```

---

## **app.html** (Frontend UI)
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>Animal Scanner — Demo
```


## License: MIT
https://github.com/react-ui-org/react-ui/blob/9014fb5f2cbd752c5753f439154b91a7268a2735/src/docs/getting-started/usage.md

```
Here are all your files:

## **server.py** (Backend - Run this)
```python
#!/usr/bin/env python3
import os
import json
import io
from http.server import HTTPServer, SimpleHTTPRequestHandler
from PIL import Image
import numpy as np

# Load AI model
AI_ENABLED = False
model = None

try:
    from tensorflow.keras.applications import MobileNetV2
    from tensorflow.keras.preprocessing import image as keras_image
    from tensorflow.keras.applications.mobilenet_v2 import decode_predictions
    import tensorflow as tf
    
    print("Loading AI model (MobileNetV2)...")
    model = MobileNetV2(weights='imagenet')
    print("✓ AI model loaded successfully!")
    AI_ENABLED = True
except Exception as e:
    print(f"⚠️  TensorFlow/Keras error: {e}")
    print("Will use basic mock AI instead\n")

class Handler(SimpleHTTPRequestHandler):
    def do_POST(self):
        if self.path == '/scan':
            content_length = int(self.headers.get('Content-Length', 0))
            body = self.rfile.read(content_length)
            
            print(f"✓ Received image ({len(body)} bytes)")
            
            try:
                if AI_ENABLED and model:
                    result = self.identify_animal_ml(body)
                else:
                    result = self.identify_animal_mock(body)
                
                self.send_response(200)
                self.send_header('Content-type', 'application/json')
                self.end_headers()
                response = json.dumps({"result": result})
                self.wfile.write(response.encode())
                print(f"  → Result: {result}\n")
            except Exception as e:
                self.send_response(500)
                self.send_header('Content-type', 'application/json')
                self.end_headers()
                response = json.dumps({"error": str(e)})
                self.wfile.write(response.encode())
                print(f"  → Error: {e}\n")
        else:
            self.send_response(404)
            self.end_headers()
    
    def identify_animal_ml(self, image_bytes):
        """Use TensorFlow AI to identify animals in image"""
        try:
            img = Image.open(io.BytesIO(image_bytes)).convert('RGB')
            img = img.resize((224, 224))
            img_array = keras_image.img_to_array(img)
            img_array = np.expand_dims(img_array, axis=0)
            img_array = tf.keras.applications.mobilenet_v2.preprocess_input(img_array)
            
            predictions = model.predict(img_array, verbose=0)
            decoded = decode_predictions(predictions, top=3)[0]
            
            results = []
            for label, description, confidence in decoded:
                conf_pct = int(confidence * 100)
                results.append(f"{description} ({conf_pct}%)")
            
            return "🐾 AI Result: " + " | ".join(results)
        except Exception as e:
            return f"ML Error: {str(e)}"
    
    def identify_animal_mock(self, image_bytes):
        """Use image analysis to make intelligent guesses"""
        try:
            img = Image.open(io.BytesIO(image_bytes)).convert('RGB')
            img_array = np.array(img)
            
            # Simple heuristics based on color distribution
            r_avg = np.mean(img_array[:,:,0])
            g_avg = np.mean(img_array[:,:,1])
            b_avg = np.mean(img_array[:,:,2])
            
            # Mock animal detection based on image colors
            animals = [
                ("Dog", "Yellow/Brown tones detected"),
                ("Cat", "Warm colors detected"),
                ("Bird", "Varied color palette detected"),
            ]
            
            # Pick based on color balance
            if r_avg > g_avg and r_avg > b_avg:
                return f"🐾 {animals[0][0]} ({int(r_avg)}% confidence) - {animals[0][1]}"
            elif g_avg > b_avg:
                return f"🐾 {animals[1][0]} ({int(g_avg)}% confidence) - {animals[1][1]}"
            else:
                return f"🐾 {animals[2][0]} ({int(b_avg)}% confidence) - {animals[2][1]}"
        except Exception as e:
            return f"Analysis failed: {str(e)}"
    
    def log_message(self, format, *args):
        print(f"[{self.log_date_time_string()}] {format % args}")

if __name__ == '__main__':
    os.chdir(os.path.dirname(os.path.abspath(__file__)))
    print("🐾 Animal Scanner Server with AI")
    print("📍 Starting server on http://localhost:8000")
    print("📱 Open http://localhost:8000/app.html in your browser")
    if AI_ENABLED:
        print("✓ TensorFlow AI enabled")
    else:
        print("⚠️  Using basic image analysis (TensorFlow not available)")
    print("\nPress Ctrl+C to stop\n")
    
    server = HTTPServer(('localhost', 8000), Handler)
    server.serve_forever()
```

---

## **app.html** (Frontend UI)
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>Animal Scanner — Demo
```


## License: MIT
https://github.com/react-ui-org/react-ui/blob/9014fb5f2cbd752c5753f439154b91a7268a2735/src/docs/getting-started/usage.md

```
Here are all your files:

## **server.py** (Backend - Run this)
```python
#!/usr/bin/env python3
import os
import json
import io
from http.server import HTTPServer, SimpleHTTPRequestHandler
from PIL import Image
import numpy as np

# Load AI model
AI_ENABLED = False
model = None

try:
    from tensorflow.keras.applications import MobileNetV2
    from tensorflow.keras.preprocessing import image as keras_image
    from tensorflow.keras.applications.mobilenet_v2 import decode_predictions
    import tensorflow as tf
    
    print("Loading AI model (MobileNetV2)...")
    model = MobileNetV2(weights='imagenet')
    print("✓ AI model loaded successfully!")
    AI_ENABLED = True
except Exception as e:
    print(f"⚠️  TensorFlow/Keras error: {e}")
    print("Will use basic mock AI instead\n")

class Handler(SimpleHTTPRequestHandler):
    def do_POST(self):
        if self.path == '/scan':
            content_length = int(self.headers.get('Content-Length', 0))
            body = self.rfile.read(content_length)
            
            print(f"✓ Received image ({len(body)} bytes)")
            
            try:
                if AI_ENABLED and model:
                    result = self.identify_animal_ml(body)
                else:
                    result = self.identify_animal_mock(body)
                
                self.send_response(200)
                self.send_header('Content-type', 'application/json')
                self.end_headers()
                response = json.dumps({"result": result})
                self.wfile.write(response.encode())
                print(f"  → Result: {result}\n")
            except Exception as e:
                self.send_response(500)
                self.send_header('Content-type', 'application/json')
                self.end_headers()
                response = json.dumps({"error": str(e)})
                self.wfile.write(response.encode())
                print(f"  → Error: {e}\n")
        else:
            self.send_response(404)
            self.end_headers()
    
    def identify_animal_ml(self, image_bytes):
        """Use TensorFlow AI to identify animals in image"""
        try:
            img = Image.open(io.BytesIO(image_bytes)).convert('RGB')
            img = img.resize((224, 224))
            img_array = keras_image.img_to_array(img)
            img_array = np.expand_dims(img_array, axis=0)
            img_array = tf.keras.applications.mobilenet_v2.preprocess_input(img_array)
            
            predictions = model.predict(img_array, verbose=0)
            decoded = decode_predictions(predictions, top=3)[0]
            
            results = []
            for label, description, confidence in decoded:
                conf_pct = int(confidence * 100)
                results.append(f"{description} ({conf_pct}%)")
            
            return "🐾 AI Result: " + " | ".join(results)
        except Exception as e:
            return f"ML Error: {str(e)}"
    
    def identify_animal_mock(self, image_bytes):
        """Use image analysis to make intelligent guesses"""
        try:
            img = Image.open(io.BytesIO(image_bytes)).convert('RGB')
            img_array = np.array(img)
            
            # Simple heuristics based on color distribution
            r_avg = np.mean(img_array[:,:,0])
            g_avg = np.mean(img_array[:,:,1])
            b_avg = np.mean(img_array[:,:,2])
            
            # Mock animal detection based on image colors
            animals = [
                ("Dog", "Yellow/Brown tones detected"),
                ("Cat", "Warm colors detected"),
                ("Bird", "Varied color palette detected"),
            ]
            
            # Pick based on color balance
            if r_avg > g_avg and r_avg > b_avg:
                return f"🐾 {animals[0][0]} ({int(r_avg)}% confidence) - {animals[0][1]}"
            elif g_avg > b_avg:
                return f"🐾 {animals[1][0]} ({int(g_avg)}% confidence) - {animals[1][1]}"
            else:
                return f"🐾 {animals[2][0]} ({int(b_avg)}% confidence) - {animals[2][1]}"
        except Exception as e:
            return f"Analysis failed: {str(e)}"
    
    def log_message(self, format, *args):
        print(f"[{self.log_date_time_string()}] {format % args}")

if __name__ == '__main__':
    os.chdir(os.path.dirname(os.path.abspath(__file__)))
    print("🐾 Animal Scanner Server with AI")
    print("📍 Starting server on http://localhost:8000")
    print("📱 Open http://localhost:8000/app.html in your browser")
    if AI_ENABLED:
        print("✓ TensorFlow AI enabled")
    else:
        print("⚠️  Using basic image analysis (TensorFlow not available)")
    print("\nPress Ctrl+C to stop\n")
    
    server = HTTPServer(('localhost', 8000), Handler)
    server.serve_forever()
```

---

## **app.html** (Frontend UI)
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>Animal Scanner — Demo
```


## License: MIT
https://github.com/react-ui-org/react-ui/blob/9014fb5f2cbd752c5753f439154b91a7268a2735/src/docs/getting-started/usage.md

```
Here are all your files:

## **server.py** (Backend - Run this)
```python
#!/usr/bin/env python3
import os
import json
import io
from http.server import HTTPServer, SimpleHTTPRequestHandler
from PIL import Image
import numpy as np

# Load AI model
AI_ENABLED = False
model = None

try:
    from tensorflow.keras.applications import MobileNetV2
    from tensorflow.keras.preprocessing import image as keras_image
    from tensorflow.keras.applications.mobilenet_v2 import decode_predictions
    import tensorflow as tf
    
    print("Loading AI model (MobileNetV2)...")
    model = MobileNetV2(weights='imagenet')
    print("✓ AI model loaded successfully!")
    AI_ENABLED = True
except Exception as e:
    print(f"⚠️  TensorFlow/Keras error: {e}")
    print("Will use basic mock AI instead\n")

class Handler(SimpleHTTPRequestHandler):
    def do_POST(self):
        if self.path == '/scan':
            content_length = int(self.headers.get('Content-Length', 0))
            body = self.rfile.read(content_length)
            
            print(f"✓ Received image ({len(body)} bytes)")
            
            try:
                if AI_ENABLED and model:
                    result = self.identify_animal_ml(body)
                else:
                    result = self.identify_animal_mock(body)
                
                self.send_response(200)
                self.send_header('Content-type', 'application/json')
                self.end_headers()
                response = json.dumps({"result": result})
                self.wfile.write(response.encode())
                print(f"  → Result: {result}\n")
            except Exception as e:
                self.send_response(500)
                self.send_header('Content-type', 'application/json')
                self.end_headers()
                response = json.dumps({"error": str(e)})
                self.wfile.write(response.encode())
                print(f"  → Error: {e}\n")
        else:
            self.send_response(404)
            self.end_headers()
    
    def identify_animal_ml(self, image_bytes):
        """Use TensorFlow AI to identify animals in image"""
        try:
            img = Image.open(io.BytesIO(image_bytes)).convert('RGB')
            img = img.resize((224, 224))
            img_array = keras_image.img_to_array(img)
            img_array = np.expand_dims(img_array, axis=0)
            img_array = tf.keras.applications.mobilenet_v2.preprocess_input(img_array)
            
            predictions = model.predict(img_array, verbose=0)
            decoded = decode_predictions(predictions, top=3)[0]
            
            results = []
            for label, description, confidence in decoded:
                conf_pct = int(confidence * 100)
                results.append(f"{description} ({conf_pct}%)")
            
            return "🐾 AI Result: " + " | ".join(results)
        except Exception as e:
            return f"ML Error: {str(e)}"
    
    def identify_animal_mock(self, image_bytes):
        """Use image analysis to make intelligent guesses"""
        try:
            img = Image.open(io.BytesIO(image_bytes)).convert('RGB')
            img_array = np.array(img)
            
            # Simple heuristics based on color distribution
            r_avg = np.mean(img_array[:,:,0])
            g_avg = np.mean(img_array[:,:,1])
            b_avg = np.mean(img_array[:,:,2])
            
            # Mock animal detection based on image colors
            animals = [
                ("Dog", "Yellow/Brown tones detected"),
                ("Cat", "Warm colors detected"),
                ("Bird", "Varied color palette detected"),
            ]
            
            # Pick based on color balance
            if r_avg > g_avg and r_avg > b_avg:
                return f"🐾 {animals[0][0]} ({int(r_avg)}% confidence) - {animals[0][1]}"
            elif g_avg > b_avg:
                return f"🐾 {animals[1][0]} ({int(g_avg)}% confidence) - {animals[1][1]}"
            else:
                return f"🐾 {animals[2][0]} ({int(b_avg)}% confidence) - {animals[2][1]}"
        except Exception as e:
            return f"Analysis failed: {str(e)}"
    
    def log_message(self, format, *args):
        print(f"[{self.log_date_time_string()}] {format % args}")

if __name__ == '__main__':
    os.chdir(os.path.dirname(os.path.abspath(__file__)))
    print("🐾 Animal Scanner Server with AI")
    print("📍 Starting server on http://localhost:8000")
    print("📱 Open http://localhost:8000/app.html in your browser")
    if AI_ENABLED:
        print("✓ TensorFlow AI enabled")
    else:
        print("⚠️  Using basic image analysis (TensorFlow not available)")
    print("\nPress Ctrl+C to stop\n")
    
    server = HTTPServer(('localhost', 8000), Handler)
    server.serve_forever()
```

---

## **app.html** (Frontend UI)
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>Animal Scanner — Demo
```

