<template>
    <div class="mo-col" :class="classes" :style="styles">
        <slot></slot>
    </div>
</template>

<script>
const prefixCls = 'mo-col';

export default {
    name: 'MoCol',
    props: {
        span: {
            type: [Number, String],
            default: 24
        },
        order: [Number, String],
        offset: [Number, String],
        push: [Number, String],
        pull: [Number, String],
        xs: [Number, Object],
        sm: [Number, Object],
        md: [Number, Object],
        lg: [Number, Object]
    },
    computed: {
        gutter() {
            let parent = this.$parent;
            while (parent && parent.$options.name !== 'MoRow') {
                parent = parent.$parent;
            }
            return parent ? parent.gutter : 0;
        },
        classes () {
            let classList = [
                `${prefixCls}`,
                {
                    [`${prefixCls}-${this.span}`]: this.span,
                    [`${prefixCls}-order-${this.order}`]: this.order,
                    [`${prefixCls}-offset-${this.offset}`]: this.offset,
                    [`${prefixCls}-push-${this.push}`]: this.push,
                    [`${prefixCls}-pull-${this.pull}`]: this.pull
                }
            ];

            ['xs', 'sm', 'md', 'lg'].forEach((size) => {
                if (typeof this[size] === 'number') {
                    classList.push(`${prefixCls}-${size}-${this[size]}`);
                } else if (typeof this[size] === 'object') {
                    let props = this[size];
                    Object.keys(props).forEach((prop) => {
                        classList.push(
                            prop !== 'span'
                                ? `${prefixCls}-${size}-${prop}-${props[prop]}`
                                : `${prefixCls}-${size}-${props[prop]}`
                        );
                    });
                }
            });

            return classList;
        },
        styles () {
            let style = {};

            if (this.gutter) {
                style.paddingLeft = `${this.gutter / 2}px`;
                style.paddingRight = style.paddingLeft;
            }

            return style;
        }
    }
};
</script>

<style>
@import '../../../style/index.css';
</style>
