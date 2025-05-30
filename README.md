# MaterialEditor Tag Generator
A Unity Editor script for automatically generating tags for AI-Shoujo / HS2 MaterialEditor.
This could make it convenient in scripting manifest.xml in a newly built shader mod.

## How to Use
1. Drag and drop the code file, **ME_Tag_Generator.cs** on the [**Release**](https://github.com/Blatke/MaterialEditor-Tag-Generator/releases) page, into **Asset/Editor/** folder in your project directory. If you don't have such an Editor folder, just create one in the Asset folder.
2. Edit your shader file by a coding tool.
3. Move to the bottom of the codes, then you see a script like _FallBack "Diffuse"_, or _FallBack "Unlit"_ or something begins with "_FallBack_". Add a new line below this "_FallBack_" and put the following script in this new line:

```
CustomEditor "ME_Tag_Generator.CustomShaderInspector"
```
So, the bottom of code now can be like:

    FallBack "Diffuse"
    CustomEditor "ME_Tag_Generator.CustomShaderInspector"

![2025-05-31_013943](https://github.com/user-attachments/assets/f0bc2dd4-2bfe-4e81-90b6-3e0e5b00ea76)

Then, save this shader file.
3. Back to Unity Editor's Project window. Select the material using your shader, and you can see a button, namely "**Generate Tags for AI/HS2 MaterialEditor**", on the Inspector window of this material.

![2025-05-31_014034](https://github.com/user-attachments/assets/583e134c-73ca-4c68-ab17-fcdb8fee2692)

4. Press this button, and it will automatically read the properties in your shader and generate tags for MaterialEditor. The tags will be saved in a new .xml file, namely _ShaderName_ME tags.xml_, created at the PATH same as that of this material. Now you've got the tags, but it still need to check:

- The attributes, Asset and AssetBundle, in a <Shader> tag will be filled like "Prefab_Name_Using_This_Shader" and "Path_of_AssetBundle_for_this_Shader", and you need to revise them to the correct values.

- The attribute DefaultValue for a Texture type <Property> tag is the texture file name of the default map set in this shader, NOT the map set in this material. If it's not assigned in this shader, its DefaultValue will remain empty.

- The attribute, DefaultValueAssetBundle, for a Texture type <Property> tag is set to "Path_of_AssetBundle_for_Tex" if it has be assigned to a default map. You need to revise it to the correct value.

- As you see, <AI_MaterialEditor> and <HS2_MaterialEditor> tags are both generated. This is for the compatibility across AI-Shoujo and HoneySelect2.

> [!TIP]
> There is a way helping to fill the Asset, AssetBundle and DefaultValueAssetBundle attributes. Please move to: [Make a Preset?](https://github.com/Blatke/MaterialEditor-Tag-Generator/blob/main/README.md#make-a-preset). 

## Supplement
### Make a Preset?
As mentioned above, the attributes such like AssetBundle or DefaultValueAssetBundle, will not be filled with the actual values since the Generator doesn't know what .unity3d files you're suppose to pack up. The guide above suggests you to manually fill them one by one and time by time as the tags are updated.

There comes a new way making it convenient since the Generator got updated to v1.0.1. The steps are listed as:
1. In the folder of the material from which you suppose to generate tags, create a .xml file as a reference and give it a name same as your shader name. Suppose the name in my shader file is "**Test/Test_for_ME_Tag_Generator**", the xml reference file can be named: **Test_for_ME_Tag_Generator**.

![2025-05-31_045746](https://github.com/user-attachments/assets/f7f1a7aa-fbef-49d7-90f4-bdef13783682)

2. Script the tags, into the reference file, that you would like the Generator transfer to the generated tag file. The <Property> tags have to be between <Shader></Shader> tags, and <Shader> tag has to be between <MaterialEditor></MaterialEditor>, <AI_MaterialEditor></AI_MaterialEditor> or <HS2_MaterialEditor></HS2_MaterialEditor>. For instance:
```
<AI_MaterialEditor>
  <Shader Name="Test/Test_for_ME_Tag_Generator" AssetBundle="xxxx" Asset="1111">
    <Property Name="MainTex" Type="Texture" DefaultValue="fff" DefaultValueAssetBundle="vfvbverer" />
    <Property Name="Fresnel" Type="Float" Range="0,1" DefaultValue="355" />
  </Shader>
</AI_MaterialEditor>
```

3. Since the name of reference file is exactly same to the shader name, the generated tags will at first check the reference file, and duplicate the <Shader> and <Property> tags that have same names as in the shader file. So the duplicates will appear in the generated tags, and for the other properties without scripted in tags in the reference file, the Generator still do its job based on the shader file.
