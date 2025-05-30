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
3. Back to Unity Editor's Project window. Select the material using your shader, and you can see a button, namely "Generate Tags for AI/HS2 MaterialEditor", on the Inspector window of this material.

![2025-05-31_014034](https://github.com/user-attachments/assets/583e134c-73ca-4c68-ab17-fcdb8fee2692)

4. Press this button, and it will automatically read the properties in your shader and generate tags for MaterialEditor. The tags will be saved in a new .xml file created in the PATH same as that of this material. Now you've got the tags, but it still need to check:

- The attributes, Asset and AssetBundle, in a <Shader> tag will be filled like "Prefab_Name_Using_This_Shader" and "Path_of_AssetBundle_for_this_Shader", and you need to revise them to the correct values.

- The attribute DefaultValue for a Texture type <Property> tag is the texture file name of the default map set in this shader, NOT the map set in this material. If it's not assigned in this shader, its DefaultValue will remain empty.

- The attribute, DefaultValueAssetBundle, for a Texture type <Property> tag is set to "Path_of_AssetBundle_for_Tex" if it has be assigned to a default map. You need to revise it to the correct value.

- As you see, <AI_MaterialEditor> and <HS2_MaterialEditor> tags are both generated. This is for the compatibility across AI-Shoujo and HoneySelect2.
