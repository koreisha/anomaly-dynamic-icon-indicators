DYNAMIC ICON INDICATORS (DII) - USER USAGE GUIDE

INTRODUCTION
• To make a proper patch for your icons/indicators to work with DII you'll need the following files.
	○ A .dds file containing your indicators, named in any way you prefer.
	○ An .xml containing the coordinates and ids for the indicators, the .xml points at the .dds of course.
	○ Two .ltx that will make sure the indicators get applied.
		► group_(anyname).ltx, this file groups up the items to use an specific indicator.
		► layer_(anyname).ltx, this file points at the id of the indicator defined at the xml and the groups you created.
	○ An .xml file containing string translations for the auto generated mcm toggles.

LONG EXPLANATION
• The .dds you make should be named any way you like, example, ui_dii_maid_indicators.dds, afterwards it should follow the next basic rules:
	○ Recommended to be in the next path [your_patch_name]/gamedata/textures/ui, and optionally you can also use [your_patch_name]/gamedata/configs/ui/[your_patch_name].
	○ Dimensions must always be a factor of 2 to be compatible with DX8, that be 32x32 32x128 64x32 64x256, factors of two being 2, 4, 8, 16, 32, 64, 128, 256, 512, 1024, 2048, 4096, 8192, etc.

• The .xml can also have any name you prefer, like the mentioned example, ui_dii_maid_indicators.xml, here you have some recommendations:
	○ It must always be in the next path [your_patch_name]/gamedata/configs/ui/textures_descr, otherwise this file will not get loaded, here's an example of how the .xml would look.
		
		Contents inside ui_dii_maid_indicators.xml >
		Do not copy the stuff inside parentheses
		
		<w>
			<file name = "ui\ui_dii_maid_indicators">	(Points at your texture file)
				<texture id = "ui_maid_icon_valuables" x="14" y="0" width="13" height="13" />	(1)(Defines the indicator location on the dds pixel by pixel, X and Y define the position, width and height define how wide or large in pixels it is, this way you cover exactly the indicator you want in specific)
			</file>
		</w>

• The group .ltx will group up the items by sections so you mass apply indicators to specific items, example, group_maid.ltx, note this is not the only way to apply indicators to items but the fastest for groups.
	○ This .ltx must be in the following location [your_patch_name]/gamedata/configs/custom_icon_layers/groups, this .ltx must always start on group_, here's an example of how the .ltx would look.
	○ Similarly to any config file in anomaly, you must define a section which can be any name, it doesn't require you to have _group nor group_ on it at all, and below it list the section of the items you want this group to contain, you can get the item's section name on the respective config file or chek it on debug menu, the tooltip on it shows it, here's an example of how the .ltx would look.
		
		Contents inside group_maid.ltx >
		Do not copy the stuff inside parentheses
		
			[valuable_maid]		(section name, aka the name of your group (2))
			money_10			(list of items by their section name, this is the 10 rubles bill)
			harmonica_a
			hand_watch
			cards
			jewelry_box
			picture_woman
			cash
			porn

• The layer .ltx will bind the .xml and the group .ltx you have created, this ltx also allows you to define single items without the need of a grop, naming example, layer_maid.ltx.
	○ This .ltx must be in the following location [your_patch_name]/gamedata/configs/custom_icon_layers/layers, this .ltx must always start on layer_.
	○ Similarly to any config file in anomaly, you must define a section which can be any name, it doesn't require you to have _layer nor layer_ on it at all, and below it you define the following information:
	
			Contents inside layer_maid.ltx >
			Do not copy the stuff inside parentheses

			► First Example
			[valuable_layer]						(section name, aka the name of your layer)
			texture = ui_maid_icon_valuables		(id of the indicator on the xml, check (1) to see how it links to the example)
			group = valuable_maid					(section name of your group, check (2) to see how it links to the example)
			anchor = right_bottom					(anchor defines one of the 4 specific positions where the indicators can show up from, these can be left_bottom, right_top, right_bottom, left_top, and only one of them)
			margin_horz = 1							(this value defines the margin left between the indicator and the horizontal limits of it, you can skip using this margin as DIO defaults to a margin of 1)
			margin_vert = 1							(this value defines the margin left between the indicator and the vertical limits of it, you can skip using this margin as DIO defaults to a margin of 1)

			► Second Example
			[maid_bottomright_base]					(you can use _base to make much cleaner sections, DIO will skip these sections and they can be used for just inheriting parameters)
			anchor = right_bottom					(in this case, anchor is gonna be inherited by many different other layer sections to make the .ltx much cleaner)

			[valuable_layer]:maid_bottomright_base	(right after the section add ":parent_section_name_base", for your layer to inherit specific parameters)
			texture = ui_maid_icon_valuables
			group = valuable_maid
			
			[parts_maid]:maid_bottomright_base
			...

			[repair_maid]:maid_bottomright_base
			...
			
			► Third Example
			[maid_attachments_base]					(3)
			settings_group = maid_attachments		(settings group can be used to superseed the group parameter on layers, this way you can make a group of groups)
			anchor = right_bottom

			[acogsight_maid]:maid_attachments_base
			texture = ui_maid_icon_acog_sight
			group = acog							(group parameter can also define an item section name directly, this is useful for single item indicators)

			[kobrasight_maid]:maid_attachments_base
			texture = ui_maid_icon_kobra_sight
			group = kobra
			
			This _base section(3) now becomes a group of groups section, in this case is containing two sights making them into a single group(4).
			
			► Fourth Example
			[maid_attachments_base]
			settings_group = maid/maid_attachments		(5)

			[acogsight_maid]:maid_attachments_base
			...

			[kobrasight_maid]:maid_attachments_base
			...
			
			(5)Using (name)/(group_of_groups_name) will make DII auto generate an mcm tab exclusive for your indicators, separating and making mcm cleaner(6).
			
• The last .xml will contain string translations for your generatd groups, make sure to group up indicators properly(5).
	○ This .xml must be in the following location [your_patch_name]/gamedata/configs/text/eng or /rus, this .xml can be named any way you prefer, for example, maid_indicators_strings.xml.
	○ You can copy paste the strings xml in this file, rename it and replace with the strings from your patch, this is the easiest way.

• Proper grouping is important(3)(4)(6) because DIO auto generates an MCM tab and toggles on it's own MCM menu, the group of groups help having your own tab for your indicators and not cluttering the general tab for the toggles.

• And that's it you have succesfully made a patch for your indicators, as of permissions, DIO is not a framework to copy paste, post your own patch and point at people to download the original mod.