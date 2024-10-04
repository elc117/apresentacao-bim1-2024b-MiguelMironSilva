##Código

```Haskell
module LoExemplo where

-- Bounding boxes: (xmin, ymin, xmax, ymax)
boundingBoxes :: [(Float, Float, Float, Float)]
boundingBoxes = [ (34.0, 60.0, 200.0, 320.0),
              	(100.0, 150.0, 250.0, 380.0),
              	(300.0, 220.0, 450.0, 450.0) ]

-- Scores: Confidence scores for each detection
scores :: [Float]
scores = [0.95, 0.80, 0.60]

-- Classes: Class 0 or 1 representing two object types
classes :: [Int]
classes = [0, 1, 0]

--Solução
width :: Float -> Float -> Float
width x1 x2 = x1 - x2

height :: Float -> Float -> Float
height y1 y2 = y1 - y2

convertBoundingBoxes :: [(Float, Float, Float, Float)] -> [(Float, Float, Float, Float)]
convertBoundingBoxes = map (\(xmin, ymin, xmax, ymax) -> (xmin, xmax, width xmax xmin, height ymax ymin))
--convertBoundingBoxes = map (\(xmin, ymin, xmax, ymax) -> (xmin, xmax, xmax - xmin, ymax - ymin))
```
