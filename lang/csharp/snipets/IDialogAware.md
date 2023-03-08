    
ビューモデルに `IDialogAware` を実装する。

`CloseCommand` のみ必須メンバーではないが、特に理由がない場合は最終的にはつけるので記述してある。  

    using System;
    using Prism.Commands;
    using Prism.Mvvm;
    using Prism.Services.Dialogs;

    namespace Filer.ViewModels
    {
        public class PageViewModel : BindableBase, IDialogAware
        {
            public event Action<IDialogResult> RequestClose;

            public string Title => string.Empty;

            public DelegateCommand CloseCommand => new DelegateCommand(() =>
            {
                RequestClose?.Invoke(new DialogResult());
            });

            public bool CanCloseDialog() => true;

            public void OnDialogClosed()
            {
            }

            public void OnDialogOpened(IDialogParameters parameters)
            {
            }
        }
    }
