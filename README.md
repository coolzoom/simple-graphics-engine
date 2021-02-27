# simple-graphics-engine
A simple 3D Graphics Engine (software rendering) developed in Visual Basic .NET in 2013 as part of the Extended Project Qualification (EPQ). Includes basic physics support.

# Functionality
## Rendering
* Wireframe rendering
* Solid-fill
* Smooth-shading
* Texture-mapping

## Other features
* Object and triangle culling
* Back face culling
* Directional colour light support

# Screenshots
![Wireframe rendering](screenshots/wireframe-sphere.png? "Wireframe rendering") | ![Smooth shading and textures](screenshots/texture-smooth-shading.png "Smooth shading and textures")
-------------------------------------------------------------|-----------------------------------------------------------------------
![Flat shaded sphere](screenshots/flat-shaded-sphere.png? "Flat shaded sphere") | ![Testing performance with many objects](screenshots/cubes.png "Testing performance with many objects")

![A test image](screenshots/bean.png? "Texture mapped cube")

# Further Work
* Use object orientation to transform the scene into an object hierarchy, rather than list of objects to render.
* Further decouple classes by using design patterns
* Replace slow memory bitmap drawing functions with [BitBlt](https://docs.microsoft.com/en-us/windows/win32/api/wingdi/nf-wingdi-bitblt).
* Use a preexisting, binary or json based file format to store models, objects
* Preprocess objects for smooth shading; for applicable models, calculate and store vertex normals.
* Implement clipping

# model format

```
line 1 name
line 2 type, 0 = coloured, 1 = textured

section 0

If Type = Triangle_Type.Coloured Then
	'If this is a coloured triangle, initialise the list of colours
	List_Colours = New List(Of Color)
ElseIf Type = Triangle_Type.Textured Then
	'If this is a textured triangle, initialise the list of textures and texture coordinates
	_Texture = Find_Texture(SR.ReadLine, List_Textures)
	List_Texture_Coords = New List(Of PointF)
End If

section 1

If Type = Triangle_Type.Coloured Then
	For i As Integer = 0 To number - 1
		'Enter all vertex indeces
		Dim V() As String = SR.ReadLine.Split(" ")

		List_Triangles.Add(New Triangle(V(0), V(1), V(2), Triangle_Type.Coloured))
		List_Colours.Add(Color.FromArgb(V(3), V(4), V(5)))
		List_Colours.Add(Color.FromArgb(V(6), V(7), V(8)))
		List_Colours.Add(Color.FromArgb(V(9), V(10), V(11)))
	Next
ElseIf Type = Triangle_Type.Textured Then
	For i As Integer = 0 To number - 1
		'Enter all vertex indeces along
		Dim V() As String = SR.ReadLine.Split(" ")

		List_Triangles.Add(New Triangle(V(0), V(1), V(2), Triangle_Type.Textured))
		List_Texture_Coords.Add(New PointF(V(3), V(4)))
		List_Texture_Coords.Add(New PointF(V(5), V(6)))
		List_Texture_Coords.Add(New PointF(V(7), V(8)))
	Next
End If

Dim number As Integer = CInt(SR.ReadLine)

If Type = Triangle_Type.Coloured Then
	For i As Integer = 0 To number - 1
		'Enter all vertices along with their colours
		Dim V() As String = SR.ReadLine.Split(" ")
		List_Vertices.Add(New VEC3(V(0), V(1), V(2)))
	Next
ElseIf Type = Triangle_Type.Textured Then
	For i As Integer = 0 To number - 1
		'Enter all vertices along with their colours
		Dim V() As String = SR.ReadLine.Split(" ")
		List_Vertices.Add(New VEC3(V(0), V(1), V(2)))
	Next
End If

number = CInt(SR.ReadLine)

If Type = Triangle_Type.Coloured Then
	For i As Integer = 0 To number - 1
		'Enter all vertex indeces
		Dim V() As String = SR.ReadLine.Split(" ")

		List_Triangles.Add(New Triangle(V(0), V(1), V(2), Triangle_Type.Coloured))
		List_Colours.Add(Color.FromArgb(V(3), V(4), V(5)))
		List_Colours.Add(Color.FromArgb(V(6), V(7), V(8)))
		List_Colours.Add(Color.FromArgb(V(9), V(10), V(11)))
	Next
ElseIf Type = Triangle_Type.Textured Then
	For i As Integer = 0 To number - 1
		'Enter all vertex indeces along
		Dim V() As String = SR.ReadLine.Split(" ")

		List_Triangles.Add(New Triangle(V(0), V(1), V(2), Triangle_Type.Textured))
		List_Texture_Coords.Add(New PointF(V(3), V(4)))
		List_Texture_Coords.Add(New PointF(V(5), V(6)))
		List_Texture_Coords.Add(New PointF(V(7), V(8)))
	Next
End If

```
### example of colored model
```
rainbowcube                                      'name
0                                                'type
8                                                'number of vertices
-0.5 -0.5 -0.5                                   'position list of vertices: x y z
-0.5 -0.5 0.5
-0.5 0.5 -0.5
-0.5 0.5 0.5
0.5 -0.5 -0.5
0.5 -0.5 0.5
0.5 0.5 -0.5
0.5 0.5 0.5
12                                              'number of triangle(vertex indeces)
0 2 4 255 0 0 0 0 255 0 255 255                 'v0 v1 v2 color_0_R color_0_G color_0_B color_1_R color_1_G color_1_B color_2_R color_2_G color_2_B 
4 2 6 0 255 255 0 0 255 255 0 128
5 4 6 255 0 255 0 255 255 255 0 128
5 6 7 255 0 255 255 0 128 128 0 255
1 0 4 0 255 0 255 0 0 0 255 255
1 4 5 0 255 0 0 255 255 255 0 255
2 3 6 0 0 255 255 255 0 255 0 128
6 3 7 255 0 128 255 255 0 128 0 255
5 3 1 255 0 255 255 255 0 0 255 0
7 3 5 128 0 255 255 255 0 255 0 255
3 2 1 255 255 0 0 0 255 0 255 0
2 0 1 0 0 255 255 0 0 0 255 0
     
0 - 255 0   0                                  'additional content, do not read, use as comment
1 - 0	255 0 
2 - 0   0   255
3 - 255 255 0
4 - 0   255 255
5 - 255 0   255
6 - 255 0   128
7 - 128 0   255
```
### example of texture model
```
facecube                                      'name
1                                             'type
face                                          'texture
8                                             'number of vertices
-0.5 -0.5 -0.5                                'position list of vertices: x y z
-0.5 -0.5 0.5
-0.5 0.5 -0.5
-0.5 0.5 0.5
0.5 -0.5 -0.5
0.5 -0.5 0.5
0.5 0.5 -0.5
0.5 0.5 0.5
12                                            'number of triangle(vertex indeces)
0 2 4 0 1 0 0 1 1                             'v0 v1 v2 texture_0_x texture_0_y texture_1_x texture_1_y texture_2_x texture_2_y
4 2 6 1 1 0 0 1 0
5 4 6 1 1 0 1 0 0
5 6 7 1 1 0 0 1 0
1 0 4 1 1 0 1 0 0
1 4 5 1 1 0 0 1 0
2 3 6 0 1 0 1 1 0
6 3 7 1 0 0 0 0 1
5 3 1 1 1 0 1 1 0
7 3 5 0 1 1 0 0 0
3 2 1 1 1 1 0 0 1
2 0 1 0 1 0 0 1 1
```

# world format
```
Dim Name As String = SR.ReadLine
Dim list_objects As New List(Of Game_Object)
Dim cam As Game_Camera

Dim model_name As String
Dim model_index As Integer
Dim model As Model

Dim position As VEC3
Dim rotation As VEC3
Dim scale As VEC3

For i As Integer = 0 To SR.ReadLine - 1 'Add objects
	model_name = SR.ReadLine
	model_index = Search_Models(model_name, List_Models)

	If model_index >= 0 Then
		model = List_Models(model_index)
	Else
		MsgBox("Could not find model file")
	End If

	position = VEC3.Parse(SR.ReadLine)
	rotation = VEC3.Parse(SR.ReadLine)
	scale = VEC3.Parse(SR.ReadLine)

	list_objects.Add(New Game_Object(model,
									 position,
									 rotation,
									 scale))
	If i = 0 Then
		cam = New Game_Camera(list_objects(i), 0)
	End If
Next
```

### example world format
```
default                        'name
5                              'number of objects
rainbowcube                    'object 0 name
0 0 -10                        'position: x y z
0 0 0                          'rotation: x y z
1 1 1                          'scale:    x y z
texturecube                    'object 1 name ...
0 0 10
0 0 0
1 1 1
facecube
2 0 10
0 0 0
1 1 1
rainbowcube
-2 0 10
0 0 0
1 1 1
beancube
0 0 0
0 0 0
1 1 1
```
