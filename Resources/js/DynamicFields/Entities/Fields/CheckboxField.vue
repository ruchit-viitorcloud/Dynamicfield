<template>
    <div class="form-group">
        <label for="cdf-{{ fieldUniqueId }}">{{ fieldInfo.name }}</label>
        <p class="text-muted" v-if="fieldInfo.instruction != ''" v-text="fieldInfo.instruction"></p>
        <!--<input id="cdf-{{ fieldUniqueId }}" class="text form-control" name="cdf[{{ fieldUniqueId }}][value]" v-model="fieldData.value" type="text">-->
        <ul class="dcf-checkbox-list dcf-bl">
            <li class="dcf-bl__li" v-for="(key, value) in fieldInfo.choices">
                <label class="dcf-bl__label">
                    <input class="form-control form-control--inline" name="cdf[{{ fieldUniqueId }}][value][]" v-model="fieldData.value" type="checkbox" value="{{ key }}" id="cdf[{{ fieldUniqueId }}][value]">
                    {{ value }}
                </label>
            </li>
        </ul>

        <input name="cdf[{{ fieldUniqueId }}][type]" v-model="fieldData.type" value="checkbox" type="hidden">
        <input name="cdf[{{ fieldUniqueId }}][field_id]" v-model="fieldInfo.unique_id" type="hidden">
        <input name="cdf[{{ fieldUniqueId }}][parent]" v-model="parentId" type="hidden">
        <input name="cdf[{{ fieldUniqueId }}][parent_layout]" v-model="parentLayout" type="hidden">
        <!--<input name="cdf[{{ fieldUniqueId }}][parent_layout]" v-model="fieldInfo.parent_layout" type="hidden">-->
    </div>
</template>

<script>
    export default {
        props : ['fieldInfo','parentLayout','parent'],
        created : function(){
            var that = this;
            if(this.fieldInfo.field_translation == undefined || this.fieldInfo.field_translation == null){
                var UniqueId = DynamicFields.GetUniqueId('field_');
                that.fieldInfo.field_translation = {
                    field_id    : that.fieldInfo.unique_id,
                    unique_id   : UniqueId,
                    value       : ''
                };
            }
        },
        ready : function(){
            if(this.fieldInfo.field_translation){
                if(this.fieldInfo.field_translation.value != null) {
                    this.fieldData.value = this.fieldInfo.field_translation.value;
                }
            }
        },
        data : function() {
            return {
                fieldData : {
                    value : [],
                    type  : 'text'
                }
            };
        },
        computed : {

            parentId : function(){
                if(this.parent != undefined){
                    return this.parent;
                }
                return 0;
            },

            theValue : function(){
                if(this.fieldInfo.field_translation){
                    if(this.fieldInfo.field_translation.value) {
                       return this.fieldInfo.field_translation.value;
                    }
                }
            },
            /*
             * so we also need a unique id for the field,
             * wether new or current
             */
            fieldUniqueId : function(){
                if(this.fieldInfo.field_translation){
                    if(this.fieldInfo.field_translation.unique_id != undefined) {
                        return this.fieldInfo.field_translation.unique_id;
                    }
                }

                return DynamicFields.GetUniqueId('field_');
            }
        }
    };
</script>

<style>

</style>
