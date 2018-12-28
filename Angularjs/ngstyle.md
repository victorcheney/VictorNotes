ng-style源码
	
	scope.$watch(attr.ngStyle, function ngStyleWatchAction(newStyles, oldStyles) {
			if (oldStyles && (newStyles !== oldStyles)) {
				forEach(oldStyles, function(val, style) {
					element.css(style, '');
				});
			}
			if (newStyles)
				element.css(newStyles);
		}, true);