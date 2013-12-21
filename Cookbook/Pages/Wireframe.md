## Fluid Powered TYPO3 Cookbook: Wireframe Page Template

> Author: Claus Due, Fluid Powered TYPO3 team. License: [GPLv3](http://www.gnu.org/copyleft/gpl.html.)

### Purpose: site wireframing without editing template files

**Not intended for production use** - designed for development stage; allows content editors to start building site content in a
blank TYPO3 site fully prepared to be switched over to use another template when it is ready.

This template is a very basic wireframe-style template which assigns a selectable number of content columns which can be used to
insert content elements, along with an extremely basic menu starting with `entryLevel="0"`. Labels of content columns can be set
and these labels are used as `data-name` attributes on the container element which gets rendered. Two additional fields allow
adding **rudimentary, kickstart-quality** styles and scripts. For ease, a checkbox is provided which when toggled on, loads the
most recent jQuery using a so-called CDN (outside hosting service, provided by Google).

When site exits wireframe stage you should disable this template to prevent it from being used on your site!

### The source

Place as any file in your Provider Extension - for example `Wireframe.html` in `Resources/Private/Templates/Page/`.

```xml
{namespace flux=FluidTYPO3\Flux\ViewHelpers}
{namespace v=Tx_Vhs_ViewHelpers}
<f:layout name="Page" />
<f:section name="Configuration">
	<flux:form id="wireframe" label="Wireframe template" icon="{f:uri.resource(path: 'Icons/Page.gif')}">
		<flux:field.checkbox name="settings.jQuery" label="Include Google-hosted jQuery library" />
		<flux:field.select name="settings.numberOfColumns" requestUpdate="TRUE" items="1,2,3,4,5,6,7,8,9"
			label="Number of content columns" />
		<v:iterator.loop count="{settings.numberOfColumns}" iteration="iteration">
			<flux:field.input name="content.{iteration.index}.label"
				label="Name, content column {iteration.index}" />
		</v:iterator.loop>
		<flux:field.text name="settings.script" label="Temporary script" cols="120" />
		<flux:field.text name="settings.style" label="Temporary styles" cols="120" />
	</flux:form>
	<flux:grid>
		<v:iterator.loop count="{settings.numberOfColumns}" iteration="iteration">
			<flux:grid.row>
				<flux:grid.column name="{iteration.index}" colPos="{iteration.index}"
					label="{v:var.get(name: 'content.{iteration.index}.label')}" />
			</flux:grid.row>
		</v:iterator.loop>
	</flux:grid>
</f:section>

<f:section name="Main">
	{v:asset.script(name: 'jQeury', path: '//ajax.googleapis.com/ajax/libs/jquery/1/jquery.min.js'
		external: 1, standalone: 1) -> f:if(condition: settings.jQuery)}
	{settings.script -> v:asset.script(name: 'temporaryScript') -> f:if(condition: settings.script)}
	{settings.style -> v:asset.style(name: 'temporaryStyle') -> f:if(condition: settings.style)}
	<v:page.menu entryLevel="0" />
	<v:iterator.loop count="{settings.numberOfColumns}" iteration="iteration">
		<div class="content-column content-column-{iteration.index}" id="content-column-{iteration.index}">
			<v:content.render column="{iteration.index}" />
		</div>
	</v:iterator.loop>
</f:section>
```
